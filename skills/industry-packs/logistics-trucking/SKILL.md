---
name: Supporting Logistics and Trucking Clients
description: Vertical pack for trucking, freight, and logistics clients — TMS platforms (McLeod, Trimble/TMW-class), ELD/telematics fleets (Samsara, Motive-class) and DOT/HOS compliance adjacency, driver devices in the field, EDI with shippers and brokers, and the 24/7 dispatch-desk rhythm. Load when the client is a carrier, broker, or 3PL, or the ticket names a TMS, ELDs, dispatch, or driver tablets.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Logistics and Trucking Clients

Freight never parks: dispatch runs around the clock, drivers are hours away from the nearest tech, and the compliance record of every driving minute lives in a federally mandated device bolted to the truck. This pack layers logistics knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework): the TMS/ELD stack, the hours-of-service records you must never touch, field-device realities, and a dispatch desk where "the board is down" means trucks and loads are flying blind.

## When to use

- The client is a trucking carrier, freight broker, 3PL, courier, or last-mile operation, or the ticket names McLeod, Trimble/TMW, TruckMate, Axon, Alvys, Turvo-class TMS platforms, or Samsara, Motive, Geotab, Omnitracs-class ELD/telematics
- "Dispatch can't see the board," "the ELD isn't logging," "drivers can't get loads on their app," EDI failures with a shipper or broker
- Driver phone/tablet issues reported from the road
- Load-board (DAT/Truckstop-class), fuel-card, or document-scanning (BOL/POD) tickets

## The stack you'll meet

- **TMS:** McLeod, Trimble/TMW, TruckMate, Axon, Alvys, Turvo-class — orders, dispatch board, driver assignments, rates, and billing live here. On-prem installs (a database server dispatch depends on) still common at mid-size carriers; newer shops are cloud. The dispatch board is the operational heartbeat.
- **ELD/telematics:** Samsara, Motive, Geotab, Omnitracs-class devices in every truck, logging hours of service under the FMCSA ELD mandate, plus GPS, dashcams, and diagnostics. Portal, gateway-device, and driver-app layers each fail differently.
- **Driver devices:** phones/tablets running the ELD app, workflow app, navigation, and document capture — used in cabs, truck stops, and dead zones. Support is remote by definition: assume intermittent connectivity, and prefer fixes the driver can do (app restart, re-pair Bluetooth to the gateway, re-login) with clear spoken-word instructions relayed through dispatch.
- **EDI and integrations:** load tenders, status updates (214s), and invoices flow via EDI/API to shippers and brokers; a silent EDI failure means missed tenders and late-status penalties. Adjacent: load boards, fuel cards, factoring portals, POD/BOL scanning, and warehouse wifi/scanners at 3PLs.

## HOS/ELD compliance — records you never touch

- ELD hours-of-service records are federal compliance data. Never edit, delete, annotate, or reconstruct HOS logs — edits are driver/carrier actions inside the vendor's certified workflow, with their own audit trail. The desk fixes devices and connectivity, never the record.
- An ELD malfunction has a regulated fallback: drivers keep paper logs per FMCSA malfunction rules for a limited period while the carrier repairs the device. Know that the fallback exists, point the safety manager to the vendor's documented malfunction procedure, and treat multi-truck ELD failures as compliance-urgent — but leave the compliance decisions (malfunction codes, reporting) to the carrier's safety owner.
- DOT audits pull ELD and maintenance records; anything that could affect record availability (portal access, data retention settings) gets the safety manager's sign-off first.

## The rhythm and what's sacred

- **24/7 dispatch:** freight moves nights and weekends; the dispatch desk is staffed accordingly and expects after-hours support paths to work. A TMS/board outage at 2 AM is not "morning queue" material.
- **Morning tender rush:** load tenders and driver check-calls cluster early morning; EDI and TMS fragility hurts most 5–9 AM.
- **Month-end billing:** invoicing and factoring submissions cluster at month-end — billing-module and document-scan failures then have cash-flow teeth.
- **Sacred:** the dispatch board (TMS availability), the ELD data pipeline, the EDI paths to top shippers, and driver communication channels. A driver out of contact with a load is the scenario dispatch fears most.

## Steps

1. Context: search_tickets for this carrier's history with the named system (ELD gateway re-pairs and EDI resends repeat), and search_itglue / search_hudu for the TMS flavor/version and hosting, ELD vendor and portal, EDI VAN/provider, top trading partners, and after-hours escalation contacts.
2. Set severity on the freight clock: whole-desk TMS/board outage or fleet-wide ELD failure → highest priority any hour of the day; single-driver device issue with the truck still moving → urgent but solvable by phone-relay steps; single-workstation issue at the office → normal.
3. For driver-device tickets, triage remotely and in order: app logged in? Bluetooth paired to the gateway? Gateway powered (truck ignition)? Cell coverage? Give dispatch a short spoken checklist for the driver; schedule physical swap at the next terminal visit if hardware is suspect.
4. Split TMS/EDI failures: local vs server vs vendor vs trading-partner side. Check vendor status pages and confirm with the partner's EDI contact before deep local debugging; document resend/reprocess steps taken.
5. Run the LOB framework for application failures: exact versions, change correlation, verbatim error, scope, vendor-escalation package with case number. ELD data problems are always vendor + safety-manager territory.
6. Note in plain text: system + versions, scope, freight-clock impact, error verbatim, driver/truck identifiers minimal (unit number over driver name), branch taken, vendor case, and verification — dispatch works a real load end to end, or the driver's next duty-status change logs cleanly.

## Guardrails

- Never edit, delete, or reconstruct HOS/ELD records or their retention settings — vendor-certified workflow and the carrier's safety owner only.
- Multi-truck ELD or dispatch-board outages are compliance/operations emergencies — escalate immediately, don't queue.
- Prefer remote, driver-executable fixes; never ask a driver to troubleshoot while driving — steps happen stopped, relayed via dispatch.
- Minimum necessary in tickets: unit numbers over driver names; no location-history dumps pasted into notes.
- EDI fixes end with confirmation from the trading partner side, not just a local "sent" status — say so if unconfirmed.
- Fuel-card and factoring-portal fraud signals (changed remittance details, unexpected card activity) → treat as a security incident via security/vendor-fraud-bec-alert, not a routine ticket.
- Docs tools vary per tenant — state what you could not verify; an undocumented EDI provider or missing after-hours path is a flag worth raising.
