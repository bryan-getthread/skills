---
name: Certificate Inventory
description: Build the expiry calendar of every certificate a client depends on — public web, RDS/gateway, LOB apps, internal CA, device certs — with ownership and the renewal runbook per cert. Use for "what certs does <client> have", "when does anything expire", or after an expired-cert outage exposes the blind spot.
category: Devices & Infrastructure
tools: [search_itglue, search_hudu, liongard_launchpoint, liongard_metric, liongard_timeline, liongard_query, search_tickets, search_knowledge_base, add_ticket_note, create_ticket, schedule_ticket]
---

# Certificate Inventory

Certificates fail loudly and on a knowable date. This skill assembles the per-client certificate register — what exists, where it lives, when it expires, who renews it and how — and turns the expiry dates into scheduled work instead of outages.

## When to use

- "What certificates does <client> have and when do they expire?"
- An expired certificate just caused an outage and leadership asks "what else is out there?"
- Quarterly hygiene: refresh the expiry calendar and book renewals.
- Onboarding a client whose certificate landscape is undocumented.

## Steps

1. Harvest documented certs: `search_itglue` / `search_hudu` for certificate, SSL/TLS, and CA documentation. Capture per cert: what it secures (public site, RDS gateway, VPN, LOB app, code signing, device/802.1X), where it is installed, issuer (public CA vs internal CA vs auto-managed like Let's Encrypt/ACME), expiry date, and who owns renewal (MSP, client, vendor, automation).
2. Harvest observed certs via Liongard where enabled — it is the best live source: `liongard_launchpoint` to confirm relevant inspectors exist and last ran successfully (TLS/SSL, IIS, Windows, domain inspectors), then `liongard_metric` / `liongard_query` for certificate names and expiry dates, and `liongard_timeline` for recent cert changes. State the dataprint age with every value taken from an inspector; skip any inspector whose last run failed and say so.
3. Sweep ticket history for the undocumented: `search_tickets` for past certificate incidents and renewals — a cert that caused a ticket two years ago is due again about now. Report capped searches as "at least N".
4. Merge into the register: cert (by what it secures — avoid pasting full subject strings when they embed internal hostnames the output does not need), location, issuer, expiry, renewal owner, renewal method, evidence source and freshness. Auto-renewing (ACME) certs are listed but marked "verify automation is alive" rather than trusted blindly — dead renewal automation is a classic silent failure.
5. Build the expiry calendar: sort by date, band into expired / <30 days / 30–90 days / later. Anything expired or <30 days becomes a ticket now (`create_ticket`); 30–90-day items get scheduled work (`schedule_ticket`) with lead time matched to the renewal method (internal-CA and vendor-managed certs need more runway than a public-CA reissue). Each ticket names the renewal owner and links the applicable runbook — `search_knowledge_base` for the desk's own procedure, and cross-reference `ssl-certificate-renewal` (troubleshooting-playbooks) for the standard web-cert path. Do not invent runbook links; if none exists, the ticket says so and requests one.
6. Output the register plus the calendar and the tickets opened, and post the plain-text summary via `add_ticket_note`. List the known-unknowns explicitly: cert classes you could not enumerate (device/802.1X fleets, LOB apps with no inspector or docs) so the register is never mistaken for complete.

## Guardrails

- Never present the register as exhaustive — certificates hide in appliances and LOB apps; always carry a known-unknowns section.
- Expiry dates come from an inspector dataprint, documentation, or a live check someone performed — never from memory or inference. Every date carries its source and freshness.
- Private keys, PFX passwords, and CA credentials are never copied into notes or the register — reference the secure store by name only.
- Renewal is a change: certs on load balancers, RDS gateways, or LOB apps can take services down when replaced — renewals for those carry a window recommendation, not a "just replace it".
- If neither Liongard nor documentation is available, build from ticket history alone, label it "reconstructed, unverified", and lead with a discovery-scan recommendation.
- Notes are plain text — no markdown, no emojis.

## See also

- `ssl-certificate-renewal` (troubleshooting-playbooks) — the renewal runbook the calendar entries point to.
- `liongard-change-review` — catching unexpected cert changes between inventory passes.
