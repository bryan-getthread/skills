---
name: Liongard Internet Domain & TLS Read
description: Answer domain, DNS, and certificate questions from Liongard's Internet Domain/DNS and TLS/SSL inspectors — registrar and expiry, DNS record changes, mail-auth records, and cert-expiry sweeps across one client or all of them.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_domain, liongard_metric, liongard_detection, liongard_timeline, liongard_query]
connectors: [Liongard]
---

# Liongard Internet Domain & TLS Read

**When to use:** "When does <client>'s domain expire / where is it registered?", "did their DNS change?" (MX/SPF/DKIM/DMARC/NS context on a mail or outage ticket), "which certificates expire in the next 30/60/90 days?" per client or across all clients, or a pre-QBR domain-hygiene check.

## Prompt

```
Answer a domain / DNS / certificate question for CLIENT_NAME (or across all clients) from the Liongard Internet Domain/DNS and TLS/SSL inspectors. Read-only.

1. liongard_launchpoint filtered by the Internet Domain/DNS and TLS/SSL systemTypes. These are external reads (no on-prem agent) and exist per monitored domain — enumerate which of the client's domains are actually watched and say so; the unwatched second domain is a standing blind spot. Verify last runs succeeded; date the dataprints. liongard_domain gives the reconciled cross-client domain inventory.
2. Route the question, verifying every JMESPath field path against the live dataprint:
   - Registration → registrar, creation/expiry dates, registrant org, name servers, registrar-lock status (angle to verify: `Domain.{registrar: Registrar, expires: ExpirationDate, locked: Status}`). Expiry within 60 days or an unlocked domain is a flagged finding with a recommended ticket. Do expiry math against today's date and show it ("expires <date>, N days out").
   - DNS state and changes → current MX, SPF, DKIM selectors, DMARC policy, NS records; liongard_detection / liongard_timeline for what changed and when. On a mail-flow ticket this is answered first. Unexpected NS or registrar changes are hijack-shaped and escalate — a hijack looks exactly like an unannounced migration.
   - Cert sweep (single client) → certs by expiry date, issuer, and covered names.
   - Cert sweep (all clients) → expiring-within-N-days across every TLS/SSL launchpoint, grouped by client, worst-first. Offer to create_ticket per expiring cert on the right client with expiry date and covered names — one ticket each, only on confirmation, deduplicated against open tickets via search_tickets first.
   - Open-ended → liongard_query ("which domains have no DMARC record?"), verified before reporting.
3. External-read caveat: these inspectors see public DNS and presented certs only — internal-only certs and split-horizon DNS are invisible; never claim full cert coverage from this sweep alone. Output: answer, evidence rows, dataprint as-of timestamps, monitored-vs-unmonitored scope statement. Plain-text notes only. Degradation: inspectors absent → public WHOIS/DNS is NOT a substitute (no instrumentation, no history) — recommend adding the inspectors and answer only what tickets/docs attest.
```
