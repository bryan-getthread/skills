---
name: Supporting Auto Dealerships
description: Vertical pack for auto-dealership clients — the DMS (CDK, Reynolds & Reynolds, Dealertrack, Tekion-class) and the 2024 CDK-outage lesson, OEM-mandated tooling and certified interfaces, the sales-floor vs service-bay split, F&I data sensitivity under the FTC Safeguards Rule, and month-end urgency. Load when the client is a dealership or the ticket names a DMS, F&I, the service lane, or OEM systems.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Auto Dealerships

A dealership is two businesses sharing one system of record: a sales floor closing deals and an F&I office moving loan applications, and a service department turning bays — all inside the DMS. The June 2024 CDK ransomware outage put thousands of dealerships on paper for weeks and made the lesson permanent: the DMS is a single point of failure you must have a downtime plan for, not just a ticket about. This pack layers dealership knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a new/used auto dealership, dealer group, powersports/RV/ag dealer, or the ticket names CDK, Reynolds & Reynolds, Dealertrack DMS, Tekion, DealerBuilt, Autosoft-class systems
- F&I platform issues (RouteOne, Dealertrack F&I-class), desking tools, credit-pull failures
- Service-lane tickets: scheduler, tablets, key machines, OEM diagnostic PCs, parts counter
- OEM-mandated tooling, certified-interface, or factory-connectivity questions
- Month-end anything at a dealership

## The stack you'll meet

- **DMS:** CDK, Reynolds & Reynolds, Dealertrack DMS, Tekion, DealerBuilt-class — inventory, deals, F&I, service ROs, parts, and accounting in one system. Mostly vendor-hosted: many "DMS is down" tickets are connectivity or vendor-side. Both CDK and Reynolds historically run through dedicated connectivity/managed network edges — know what the vendor controls at this site before touching network gear.
- **OEM layer:** each franchise brand mandates tools — OEM DMS-certified interfaces, warranty-claim portals, diagnostic laptops in service (vendor/OEM-managed; treat like vendor-locked instruments), OEM VPN appliances. OEM-mandated devices and their network segments follow OEM rules; changes there can threaten franchise compliance — coordinate with the dealer principal/fixed-ops director, don't improvise.
- **Sales floor vs service bay:** the sales side lives in CRM (VinSolutions, DealerSocket, Elead-class), desking, and F&I; the service side lives in the scheduler, lane tablets, multipoint-inspection apps, and parts. They peak at different hours and escalate through different managers — know which side a ticket belongs to and who owns it.
- **Adjacent:** credit-pull and e-contracting paths (RouteOne/Dealertrack), plate/registration systems, key-cutting machines, sales-floor wifi (guest-heavy), and lots of printers — window stickers, buyers guides, RO printers in the shop.

## F&I data and the Safeguards Rule

Dealers arrange financing, which makes them "financial institutions" under the FTC Safeguards Rule — customer credit applications, SSNs, and deal files are regulated data.

- Minimum necessary in tickets: no screenshots of deal screens or credit apps, no customer identity paired with financing details. "E-contracting errors on all deals" beats naming the buyer.
- F&I workstations and the paths credit data travels are the dealership's compliance surface — changes there (new software, remote-access tools, print-to-PDF flows) deserve the compliance owner's awareness.
- Suspected exposure of credit-application data → capture facts, flag the dealership's compliance owner and your internal escalation path; no legal opinions.

## The rhythm and what's sacred

- **Month-end:** the last three business days of the month are the sales push — deals, desking, F&I, and DMS accounting are all peak-fragility. No discretionary changes to the DMS path, F&I tools, or e-contracting then; a broken credit pull on the 31st is a top-severity, manager-notified event.
- **Saturday:** the busiest sales day, with skeleton IT-vendor coverage — sales-floor and F&I failures on Saturday carry weekend urgency.
- **Service mornings:** the service lane writes most of its ROs 7–9 AM; scheduler or lane-tablet failure at opening backs up the drive.
- **Sacred:** the DMS path (connectivity + vendor edge), F&I/e-contracting during selling hours, and the DMS-down plan itself. After CDK 2024, a dealership without a documented paper-fallback plan (deal jackets, manual ROs, parts tickets) is carrying unpriced risk — flag its absence.

## Steps

1. Context: search_tickets for this dealer's history with the named system, and search_itglue / search_hudu for the DMS flavor and vendor-managed edge, OEM brands and mandated tooling, F&I platforms, and the DMS-downtime plan location if one exists.
2. Set severity on the dealership clock: whole-store DMS or F&I outage during selling hours (worst: month-end) → highest priority, dealer principal or GM notified; single-workstation or single-printer issue → normal with a workaround stated.
3. Split the failure domain: local workstation/LAN vs the vendor-managed connectivity edge vs vendor-side outage. Check the DMS vendor's status early — and remember much of the network path may be the vendor's to fix, not yours; open their case and say so.
4. Run the LOB framework for application failures: exact versions, change correlation, verbatim error, scope, vendor-escalation package with case number. OEM-managed devices and segments → coordinate with the OEM process via the fixed-ops/principal contact rather than direct surgery.
5. If the DMS outage looks extended (vendor incident, ransomware event upstream): surface the dealership's documented downtime plan immediately; if none exists, flag that gap to the account owner as its own follow-up — during the incident, help the managers fall back in an orderly way rather than inventing process mid-crisis.
6. Note in plain text: system + versions, scope, dealership-clock impact, error verbatim (customer-data-scrubbed), boundary handoffs (DMS vendor / OEM), case numbers, and verification by a user running a real deal, RO, or parts transaction.

## Guardrails

- Never modify vendor-managed DMS connectivity edges or OEM-mandated devices/segments outside the vendor/OEM process — franchise compliance and support standing are at stake.
- No customer credit or deal data in tickets beyond minimum necessary; no deal-screen screenshots; suspected exposure → facts only, flag the compliance owner.
- Month-end selling days are a change-freeze window for the DMS, F&I, and e-contracting paths; discretionary work waits.
- Ransomware-shaped signals at a dealership (DMS vendor incident, encryption notes, mass lockouts) → run the security/ransomware-response path immediately; the 2024 lesson is that dealer outages can be someone else's breach — verify which side of the boundary the incident is on before acting.
- Do not fabricate or improvise a downtime process during an outage; surface the documented plan or escalate its absence.
- Docs tools vary per tenant — state what you could not verify, and flag a missing DMS-down plan or undocumented OEM tooling as follow-ups.
