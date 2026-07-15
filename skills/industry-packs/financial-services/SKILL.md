---
name: Supporting Financial Services Clients
description: Vertical pack for advisory, broker-dealer, banking, and insurance clients — FINRA/SEC retention and archiving obligations (never break the journal), heightened change control, the advisory/wealth stack (Orion, Redtail-class), and market-hours urgency. Load when the client is an RIA, broker-dealer, bank, credit union, or insurance agency, or the ticket touches email archiving, retention, or trading systems.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Financial Services Clients

At a financial-services client, the regulator is a silent third party on every ticket. Email is not just email — it is a books-and-records artifact that must be captured, archived immutably, and producible for years; a change is not just a change — it may need compliance sign-off; and a routine mailbox cleanup can become a regulatory violation. This pack layers financial-services knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a registered investment advisor (RIA), broker-dealer, bank, credit union, wealth-management firm, or insurance agency, or the ticket names Orion, Black Diamond, Tamarac, Redtail, Wealthbox, Smarsh, Global Relay, or a trading/custodial platform
- Anything touching email flow, mailboxes, retention, journaling, archiving, or messaging apps at such a client
- Offboarding, mailbox deletion, device wipes, or data-disposal requests (retention check first)
- Change requests at a client with a compliance officer

## The compliance regime: retention is physics here

- **Books and records:** broker-dealers live under SEC 17a-4-style retention (immutable, multi-year, promptly producible); RIAs under the Advisers Act books-and-records rules; banks/insurers under their own regimes. Practical translation for the desk: **business communications must be captured and archived — and the desk must never create a gap.**
- **The journal is sacred.** Email typically flows through journaling/capture into an archive (Smarsh, Global Relay, Proofpoint-class, or M365 compliance features). The standing rules:
  - Never disable, bypass, or "temporarily pause" journaling, transport rules feeding the archive, or retention policies — not as a troubleshooting step, not during a migration, not briefly. Mail-flow changes at these clients are checked against the capture path *before* and verified *after*.
  - Never delete mailboxes, purge items, shrink retention, or wipe devices holding business communications without the firm's compliance officer signing off in writing — departures included. Offboarding here preserves first, disables access second, deletes (maybe) much later.
  - Legal/regulatory hold interplay works like law firms — see onboarding-and-access/litigation-hold; regulatory examinations create the same do-not-destroy posture.
- **Off-channel communications** are a live enforcement topic: firms are obligated to keep business comms on captured channels. The desk's role is never to *enable* workarounds (installing unarchived messaging apps for business use, forwarding to personal email) and to flag requests that would create off-channel flows to the compliance officer rather than fulfilling them quietly.
- **Heightened change control:** many firms require compliance/principal review for changes touching communications, client data, or access. Know each firm's documented approval path; when a change smells regulated and no path is documented, ask the compliance officer first. "Flag to the compliance owner" is the default move for anything ambiguous.

## The stack you'll meet

- **Advisory/wealth:** portfolio management and reporting (Orion, Black Diamond, Tamarac-class), CRM (Redtail, Wealthbox, Salesforce Financial Services-class), financial planning (eMoney, RightCapital-class), custodial portals (Schwab, Fidelity-class) — mostly cloud; tickets are SSO, integration-sync, and browser problems, with data-feed failures (overnight custodial downloads) as the recurring morning fire: a failed feed means advisors open the day with stale portfolios — treat as multi-user impact.
- **Archiving/supervision:** Smarsh, Global Relay-class, or M365-native retention/eDiscovery — the desk supports the plumbing, the firm's compliance staff runs supervision.
- **Broker-dealer/trading:** order-management and trading platforms, market-data terminals (Bloomberg-class). During market hours these are the line-down equivalent.
- **Banking/insurance:** core-banking or agency-management systems (Applied Epic, Vertafore-class for insurance) — classic LOB with vendor-escalation discipline, plus regulator-driven security postures (MFA everywhere, encryption, vendor-management questionnaires the MSP will be asked to answer — route those to the account owner, answer honestly, never aspirationally).

## The rhythm and what's sacred

- **Market hours (9:30 AM–4:00 PM ET) are the urgency window** for anything touching trading, market data, or advisor workstations — a trading-desk problem at 3:45 PM ET is a sprint. Maintenance lives outside market hours and outside the overnight data-feed window (learn when the feeds run; a reboot at 2 AM can be worse than one at 2 PM).
- **Quarter-end** brings client reporting and billing runs on the portfolio platform — freeze-adjacent care. Tax season raises the temperature at wealth firms too (1099 and reporting queries).
- **Sacred:** the archive/journal path (above all), the overnight data feeds, trading/market access during market hours, and the client-data boundary — client financial information gets the same never-in-tickets hygiene as PHI: no account numbers, balances, holdings, or client identities paired with financials in notes or screenshots.

## Steps

1. Context: search_tickets for this firm + system history, and search_itglue / search_hudu for the documentation: regulatory profile (BD vs RIA vs bank vs insurance), archiving vendor and capture path, compliance officer contact, documented change-approval path, data-feed schedule, support contracts.
2. Regulated-surface screen: does the ticket touch email flow, retention, mailbox lifecycle, messaging apps, or client-data movement? If yes → identify the compliance owner and the approval requirement *before* acting; capture the sign-off in the ticket.
3. Triage on the market clock: trading/market-data down in market hours or firm-wide platform outage → top severity; failed overnight feed discovered at 7 AM → resolve or vendor-escalate before market open; single-user off-hours → normal flow.
4. Run the LOB framework for platform failures: exact versions/tenant, change correlation, verbatim error, vendor status pages early for the cloud stack; data-feed failures get diagnosed at the integration layer (credentials expiry, custodian-side changes, file-drop failures) with the platform vendor's known-issue base searched before deep local work.
5. Vendor territory → complete vendor-escalation package with case number; note market-hours or quarter-end deadlines in the case. Archiving-vendor issues get flagged to the compliance officer in parallel — a capture gap is their reportable problem and the clock matters.
6. For offboarding/deletion/wipe requests: preserve → confirm compliance sign-off in writing → disable access → only then any destruction, per the firm's retention schedule. Record the approval chain in the ticket.
7. Note in plain text, client-financials-scrubbed: system, regulated-surface determination, approvals obtained, market-clock context, error verbatim, branch, vendor case, verification — including post-change confirmation that journaling/capture is intact whenever mail flow was touched.

## Guardrails

- Never disable, bypass, or pause journaling, archive transport rules, or retention policies — for any reason, for any duration — without the compliance officer's explicit written direction. Verify capture integrity after any mail-flow change.
- No mailbox deletion, item purge, retention shortening, or device wipe holding business comms without written compliance sign-off; departures preserve first. Cross-ref onboarding-and-access/litigation-hold for hold interplay.
- Never install or configure unarchived communication channels for business use, or forwardings to personal accounts — requests that would create off-channel comms go to the compliance officer, not into fulfillment.
- Client financial data stays out of tickets: no account numbers, balances, or client-identity-plus-financials pairings; minimum necessary.
- Changes on regulated surfaces follow the firm's documented approval path; undocumented + ambiguous = ask the compliance officer first. No regulatory or legal advice — factual flags only.
- Respect the market clock and feed windows for anything disruptive; quarter-end gets freeze-adjacent caution on reporting/billing platforms.
- Vendor-due-diligence questionnaires about the MSP's own controls route to the account owner and are answered accurately, never aspirationally.
- Docs tools vary per tenant — state what you could not verify; a financial-services client with no documented compliance contact or capture path is itself an urgent flag for the account owner.
