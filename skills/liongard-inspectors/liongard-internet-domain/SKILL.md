---
name: Liongard Internet Domain & TLS Read
description: Answer domain, DNS, and certificate questions from Liongard's Internet Domain/DNS and TLS/SSL inspectors — registrar and expiry, DNS record changes, mail-auth records, and cert-expiry sweeps across one client or all of them.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_domain, liongard_metric, liongard_detection, liongard_timeline, liongard_query, search_tickets, add_ticket_note, create_ticket]
---

# Liongard Internet Domain & TLS Read

Domains and certificates fail loudly and get renewed quietly — the Internet Domain/DNS and TLS/SSL inspectors are the cheap external watch on both. This skill answers "when does it expire, where is it registered, what changed in DNS, and which certs die next" — for one client on a ticket, or swept across every client before the next preventable outage.

## When to use

- "When does <client>'s domain expire / where is it registered / who's the registrar?"
- "Did their DNS change?" — MX, SPF, DKIM, DMARC, NS record change context on a mail or outage ticket
- "Which certificates expire in the next 30/60/90 days?" — per client or across all clients
- Pre-QBR domain-hygiene check, or evidence for security/typosquat-domain-alert baselines

## Steps

1. Base pattern (skills/liongard-inspectors/liongard-access-pattern/SKILL.md): liongard_launchpoint filtered by the Internet Domain/DNS and TLS/SSL systemTypes. These inspectors are external reads (no agent on-prem), so they exist per monitored domain — enumerate which of the client's domains are actually watched and say so; the unwatched second domain is a standing blind spot. Verify last runs succeeded; date the dataprints. liongard_domain gives the reconciled cross-client domain inventory.
2. Route the question:
   - Registration → registrar, creation/expiry dates, registrant org, name servers, registrar-lock status where captured. Angle shape: `Domain.{registrar: Registrar, expires: ExpirationDate, locked: Status}` — verify field paths against the live dataprint. Expiry within 60 days, or an unlocked domain, is a flagged finding with a recommended ticket.
   - DNS state and changes → current MX, SPF, DKIM selectors, DMARC policy, NS records; liongard_detection / liongard_timeline for what changed and when. On a mail-flow ticket this is the first question answered: "SPF record changed <date>" turns a mystery into a change review. Mail-auth failures themselves route to security/dmarc-spf-failure-triage; unexpected NS or registrar changes are hijack-shaped and escalate to security review.
   - Cert sweep (single client) → certs by expiry date, issuer, and covered names. Renewal execution belongs to troubleshooting-playbooks/ssl-certificate-renewal and the tracked inventory to devices-and-infrastructure/certificate-inventory — this read feeds both.
   - Cert sweep (all clients) → the cross-client move: expiring-within-N-days across every TLS/SSL launchpoint, grouped by client, worst-first. Offer to create_ticket per expiring cert on the right client with the expiry date and covered names — one preventable-outage ticket each, only on confirmation.
   - Open-ended → liongard_query ("which domains have no DMARC record?"), verified before reporting.
3. Output: answer, evidence rows, dataprint as-of timestamps, monitored-vs-unmonitored domain scope statement. Plain-text notes; no markdown in PSA-bound output.

## Guardrails

- External-read caveat: these inspectors see public DNS and presented certs — internal-only certs and split-horizon DNS are invisible; never claim full cert coverage from this sweep alone (devices-and-infrastructure/certificate-inventory holds the fuller picture).
- Expiry math is done against today's date and shown ("expires <date>, N days out") — never just "expiring soon."
- Registrar/NS changes without a matching change ticket escalate rather than report — domain hijacks look exactly like unannounced migrations.
- Ticket creation only on confirmation, one per finding, deduplicated against existing open tickets (search_tickets first).
- Degradation: inspectors absent → answer from public WHOIS/DNS knowledge is NOT a substitute (no instrumentation, no history) — recommend adding the inspectors and answer only what tickets/docs attest.
