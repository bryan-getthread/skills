---
name: Certificate Inventory
description: Build the expiry calendar of every certificate a client depends on — public web, RDS/gateway, LOB apps, internal CA, device certs — with ownership and renewal runbook per cert. Use for "what certs does <client> have", "when does anything expire", or after an expired-cert outage.
category: Devices & Infrastructure
tools: [search_itglue, search_hudu, liongard_launchpoint, liongard_metric, liongard_timeline, liongard_query, search_tickets, search_knowledge_base, add_ticket_note, create_ticket, schedule_ticket]
connectors: [Liongard, IT Glue, Hudu]
---

# Certificate Inventory

**When to use:** "What certificates does <client> have and when do they expire?" — or an expired cert just caused an outage and leadership asks "what else is out there?"

## Prompt

```
Assemble the per-client certificate register — what exists, where it lives, when it expires, who renews it and how — and turn expiry dates into scheduled work instead of outages.

1. Harvest documented certs: search_itglue / search_hudu for certificate, SSL/TLS, and CA docs. Per cert capture: what it secures (public site, RDS gateway, VPN, LOB app, code signing, device/802.1X), where installed, issuer (public CA vs internal CA vs ACME/Let's Encrypt), expiry date, renewal owner (MSP/client/vendor/automation).
2. Harvest observed certs via Liongard where enabled (best live source): liongard_launchpoint to confirm relevant inspectors exist and last ran successfully (TLS/SSL, IIS, Windows, domain), then liongard_metric / liongard_query for cert names and expiry dates, and liongard_timeline for recent cert changes. State the dataprint age with every inspector value; skip any inspector whose last run failed and say so.
3. Sweep ticket history for the undocumented: search_tickets for past cert incidents/renewals (a cert that caused a ticket ~2 years ago is due again about now). Report capped searches as "at least N".
4. Merge into the register: cert (by what it secures — avoid pasting full subject strings that embed internal hostnames), location, issuer, expiry, renewal owner+method, evidence source and freshness. List auto-renewing (ACME) certs but mark "verify automation is alive" rather than trusting blindly.
5. Build the expiry calendar: sort by date; band into expired / <30d / 30-90d / later. Expired or <30d -> create_ticket now; 30-90d -> schedule_ticket with lead time matched to renewal method (internal-CA and vendor-managed need more runway than a public-CA reissue). Each ticket names the renewal owner and links the applicable runbook (search_knowledge_base for the desk's procedure). Do not invent runbook links; if none exists, the ticket says so and requests one.
6. Output the register + calendar + tickets opened, and post the plain-text summary via add_ticket_note (no markdown/emojis). Include a known-unknowns section: cert classes you could not enumerate (device/802.1X fleets, LOB apps with no inspector or docs), so the register is never mistaken for complete.

Guardrails: never present the register as exhaustive — certs hide in appliances and LOB apps. Every expiry date carries its source and freshness — never from memory or inference. Private keys, PFX passwords, and CA credentials are never copied into notes or the register — reference the secure store by name only. Renewals on load balancers / RDS gateways / LOB apps can take services down — those carry a window recommendation, not "just replace it". If neither Liongard nor docs are available, build from ticket history alone, label it "reconstructed, unverified", and lead with a discovery-scan recommendation.
```
