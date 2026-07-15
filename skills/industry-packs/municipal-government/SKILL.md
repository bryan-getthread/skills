---
name: Supporting Municipal Government
description: Vertical pack for city/town/county and special-district clients — public-records retention (email is a public record), CJIS awareness for anything PD-adjacent, procurement and budget-cycle realities, and public-meeting AV. Load when the client is a municipality, utility district, or public agency, or the ticket touches records requests, police systems, or council meetings.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Municipal Government

A municipality is a business where every email might be read by the public, some systems require a background check to touch, buying anything takes a purchase order and possibly a board vote, and the most visible IT failure of the year is a council meeting the livestream missed. This pack layers public-sector knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a city, town, county, village, utility district, housing authority, library, or other public agency
- Anything touching email retention, deletion, mailbox lifecycle, or messaging at a public agency (public-records check)
- Any work adjacent to police, dispatch, courts, or systems holding criminal-justice information (CJIS gate)
- Quotes, purchases, or projects at a municipal client (procurement realities), or council/board meeting AV

## The compliance regime

### Public records: assume everything is producible

- State public-records / FOIA-style laws make emails, texts about public business, and many documents **public records** with retention schedules set by state law and producible on request. Desk translation:
  - Never delete mailboxes, purge items, shorten retention, or wipe devices at a public agency without the records officer's (often the clerk's) sign-off — departures included; the departed official's mailbox is a records archive, not cleanup fodder.
  - Records requests and litigation create hold conditions exactly like the private sector — cross-ref onboarding-and-access/litigation-hold; an open records request over a topic means nothing touching that topic gets destroyed.
  - Don't enable off-the-record channels: auto-delete chat settings, personal-email forwarding for officials, or ephemeral messaging for public business are requests to flag to the clerk/records officer, not fulfill.
- The desk gives no opinions on what is or isn't a public record — that is the records officer's and the municipal attorney's call. Flag and route.

### CJIS: the do-not-touch-without-clearance zone

- Anything holding **criminal justice information** — police RMS/CAD, evidence systems, mobile data terminals in patrol cars, interfaces to state/NCIC systems — falls under the **CJIS Security Policy**: personnel need fingerprint-based background checks and CJIS security-awareness training before *unescorted access* (physical or remote) to CJI systems, and the MSP typically must have a CJIS Security Addendum in place with the agency.
  - **The rule:** no tech touches PD/dispatch-adjacent systems without confirmed CJIS clearance for that individual at that agency. Unknown clearance status = stop, and route to the agency's Terminal Agency Coordinator (TAC) or LASO.
  - This includes indirect paths: the backup system that includes the RMS server, the RMM agent on a dispatch workstation, the network gear segmenting the PD — scope carefully and document who may touch what.
  - CJI never appears in tickets: no case data, no names from police systems, nothing — describe systems, not contents.

## The stack you'll meet

- **Core:** municipal ERP/finance (Tyler-class — Munis, Incode —, Springbrook, BS&A-class): utility billing, permits, payroll; classic LOB with vendor-escalation discipline, sacred on utility-billing and payroll runs.
- **Public safety (CJIS-gated):** CAD/RMS (Tyler, Motorola, Central Square-class), bodycam/evidence platforms (Axon-class), MDTs in vehicles.
- **Clerk/meetings:** agenda-management and streaming (Granicus, CivicPlus-class, or Zoom/YouTube rigs), council-chamber AV (mics, switchers, recording — the recording itself is often a required record).
- **Also:** SCADA at water/wastewater utilities (full OT rules apply — see the manufacturing pack's OT/IT boundary: never touch plant control systems without the utility's operations owner), library systems, recreation/permit portals, and aging on-prem everything — municipalities run long refresh cycles by budget necessity.

## Procurement and the budget cycle

- Municipalities buy through **purchase orders, spending thresholds, and formal processes**: above certain amounts, quotes or sealed bids and board approval are required by law; splitting purchases to duck thresholds is illegal — never suggest it. Quotes should be written knowing they may enter a public agenda packet.
- **The fiscal year (commonly July 1, sometimes Jan 1 or Oct 1)** governs everything: budget requests happen months ahead, unspent funds may lapse (a use-it-or-lose-it flurry near year-end), and mid-year emergencies need special approvals. Plan hardware refreshes and projects around the budget calendar with the client, not around the desk's convenience.
- Timeline honesty: even an approved purchase can take weeks through PO issuance — set expectations accordingly, and flag genuinely urgent replacements to the client's finance officer early.

## The rhythm and what's sacred

- **Meeting nights are sacred.** Council/board meetings (often specific weekday evenings) require the chamber AV, streaming, and agenda systems working — a failed livestream can raise open-meetings compliance questions, which are the clerk's and attorney's to handle, and the desk's to prevent. Pre-flight the chamber before meetings; freeze changes to the meeting stack on meeting days.
- **Dispatch/public-safety is 24/7** and outages there are top severity by definition. **Utility operations** (water/wastewater SCADA) similarly — with the OT boundary respected.
- Elections (where the client has any election role) create absolute freeze windows around election dates — election systems themselves are usually county/state-run; know the boundary and stay on the right side of it.
- **Sacred:** public-safety systems, the records/retention integrity of email and documents, utility billing and payroll runs, meeting-night AV, and SCADA at utilities.

## Steps

1. Context: search_tickets for this agency + system history, and search_itglue / search_hudu for the documentation: records officer and retention schedule location, CJIS scope and cleared-personnel list, TAC/LASO contact, meeting calendar, fiscal year, procurement thresholds, vendor contracts.
2. Gate check before touching anything: CJIS-adjacent → confirm the assigned tech's clearance at this agency or stop and route to the TAC/LASO; records-touching (deletion, retention, mailbox lifecycle) → records-officer sign-off first; SCADA-adjacent → operations owner per the OT boundary.
3. Triage on the civic clock: dispatch/public-safety down → top severity any hour; meeting-stack issues on a meeting day → urgent with pre-meeting deadline; utility-billing or payroll failures near their runs → elevated; single-user → normal.
4. Run the LOB framework for application failures: exact versions, change correlation, verbatim error, vendor known-issue search, and vendor-escalation packages through the agency's support contracts (municipal vendors are used to formal cases — completeness pays off doubly here); note meeting/billing deadlines in the case.
5. For purchases: provide written quotes suitable for public packets, flag threshold implications to the client's finance officer factually ("this amount may require board approval per your policy — your finance office can confirm"), and never structure around thresholds.
6. Note in plain text, CJI-and-resident-data-free: system, gates checked and approvals obtained, civic-clock context, error verbatim, branch, vendor case, verification by staff performing the real workflow (run a test bill, cut a test agenda packet, confirm the stream).

## Guardrails

- No unescorted access to CJIS-scope systems by uncleared personnel — unknown clearance = stop and route to the TAC/LASO. CJI never appears in tickets.
- No deletion, purge, retention change, or wipe at a public agency without the records officer's sign-off; open records requests and litigation create holds (cross-ref onboarding-and-access/litigation-hold). Never enable off-the-record communication channels for public business.
- SCADA and plant control systems follow the full OT boundary: coordinate with the utility's operations owner; never scan, patch, or reboot uninvited.
- Meeting-day freeze on the chamber/streaming stack; pre-flight instead. Election-window freezes are absolute.
- Procurement: written quotes, factual threshold flags, and zero tolerance for purchase-splitting suggestions. Timeline honesty about PO cycles.
- No legal opinions — on records status, open-meetings law, CJIS interpretation, or anything else; the clerk, attorney, and TAC own those calls. Flag and route.
- Resident data (utility accounts, permits, court records) gets minimum-necessary ticket hygiene like any regulated vertical.
- Docs tools vary per tenant — state what you could not verify; an agency with no documented CJIS scope or records officer is itself an urgent flag for the account owner.
