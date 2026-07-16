---
name: Supporting Pharmacies
description: Vertical pack for independent and small-chain pharmacy clients — the pharmacy management system (PioneerRx, Rx30, Computer-Rx-class), the claims-adjudication switch, e-prescribing and EPCS, HIPAA and controlled-substance adjacency (DSCSA, DEA records), IVR/POS integrations, and the "system down = can't dispense" urgency model. Load when the client is a pharmacy or the ticket names a dispensing system, the switch, or refill IVR.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Pharmacies

A pharmacy that can't run its dispensing system can't legally or safely fill prescriptions — there is no paper fallback for adjudicating insurance, checking interactions, or logging a controlled substance. "System down" here means patients standing at the counter without their medication. This pack layers pharmacy-specific knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework): the dispensing stack, the regulated records you must never touch, and an urgency model where dispensing outages outrank almost everything.

## When to use

- The client is an independent pharmacy, small chain, LTC pharmacy, or compounding pharmacy, or the ticket names PioneerRx, Rx30, Computer-Rx, Liberty, BestRx, QS/1, McKesson EnterpriseRx-class systems
- "Claims aren't going through," "e-scripts aren't coming in," "the IVR is down," "we can't scan at pickup"
- Anything adjacent to controlled-substance records, EPCS tokens/authenticators, or DSCSA scanning
- Robot/counting-machine, label-printer, or POS/register tickets at a pharmacy

## The stack you'll meet

- **Pharmacy management system (PMS/dispensing):** PioneerRx, Rx30, Computer-Rx, Liberty, BestRx, QS/1-class — patient profiles, prescriptions, dispensing log, controlled-substance records, and inventory live here. On-prem installs mean a server in the back; cloud/hosted installs shift failures to connectivity.
- **The switch:** insurance claims adjudicate in real time through a claims switch between pharmacy and PBMs. When the switch or its upstream is down, the pharmacy can dispense but not bill — the 2024 Change Healthcare outage showed this can last weeks, so know whether the dispensing system supports an alternate switch route or offline/bill-later mode, and where that procedure is documented.
- **E-prescribing:** inbound scripts arrive via Surescripts-class networks; EPCS (controlled-substance e-prescribing) adds two-factor hardware/app tokens on the prescriber side. "No new e-scripts for an hour" is a queue/connectivity ticket with real clinical urgency.
- **Integrations:** refill IVR phone lines (patients phone in refills 24/7), POS/registers with signature capture and DSCSA barcode scanning at pickup, counting robots (Parata/ScriptPro-class — vendor-maintained; treat like lab instruments), delivery apps, wholesaler ordering (order cutoff times matter — a missed cutoff is a next-day stock gap).

## Regulated records — look, don't touch

- **HIPAA:** the pharmacy is a covered entity; the MSP is a business associate. Minimum necessary in every artifact — no patient names paired with medications in tickets, no dispensing-screen screenshots. "Profile lookups error for all patients" beats naming one.
- **Controlled substances:** DEA recordkeeping and state boards govern the CS dispensing records and perpetual inventory inside the PMS. Never edit, correct, delete, or reconstruct controlled-substance records or inventory counts — not even at the pharmacist's casual request. Data problems in those records go through the vendor's documented procedure with the pharmacist-in-charge's involvement; the desk's role is facilitation, never data surgery.
- **DSCSA:** track-and-trace scanning at receipt is a compliance function. A broken scanner or verification-service failure blocks receiving product — flag it as compliance-relevant, fix the hardware/connectivity side, leave the compliance decisions to the pharmacist-in-charge.
- Suspected PHI exposure → capture facts, flag the pharmacy's compliance/privacy owner and your internal escalation path; no breach-notification opinions.

## The rhythm and what's sacred

- **Peak windows:** opening hour (overnight e-script and IVR queue processed at once), lunchtime pickup, and the 4–6 PM post-work rush. Month boundaries add refill-cycle and LTC cycle-fill load.
- **The waiting patient:** any whole-pharmacy dispensing, e-script, or adjudication outage during open hours is top severity, full stop — patients may be waiting on urgent medication. Communicate honest workaround status early.
- **Sacred:** the dispensing system and its database/backup, the switch path, inbound e-prescribing, and the controlled-substance records. The IVR is next — it is the refill intake pipe.

## Steps

1. Context: search_tickets for this pharmacy's history with the named system (adjudication hiccups and IVR resets repeat), and search_itglue / search_hudu for the PMS flavor and version, switch vendor, IVR vendor, wholesaler cutoffs, vendor support contracts and portal-credential locations.
2. Set severity on the dispensing clock: can they fill and bill right now? Whole-store dispensing/adjudication/e-script outage during open hours → highest priority and immediate human dispatch. Single-register or single-workstation issue with others working → normal, with a stated workaround.
3. Split the failure domain fast: local (workstation, LAN, printer, scanner) vs dispensing server vs internet/VPN vs upstream (switch, Surescripts, PBM). Check the vendor and switch status pages early — upstream outages are common and the honest answer is "vendor-side, case opened, here is the offline procedure if documented."
4. Run the LOB framework: exact versions, change correlation, verbatim error, scope. Anything inside the PMS database — and *always* anything touching controlled-substance data — is vendor territory: build the escalation package, involve the pharmacist-in-charge, log the case number.
5. For billing outages with a documented offline/bill-later mode: point the pharmacist to the pharmacy's own documented procedure; do not improvise a billing workaround.
6. Note in plain text: system + versions, scope, dispensing-clock impact, error verbatim (PHI-scrubbed), branch taken, vendor case number, and verification — the pharmacist fills and adjudicates a real (test or next live) prescription end to end.

## Guardrails

- Never edit, delete, or reconstruct controlled-substance records, CS inventory counts, or dispensing-log entries — vendor procedure + pharmacist-in-charge only, no exceptions.
- Never operate directly on the PMS database outside vendor-documented procedure.
- Minimum-necessary PHI; no dispensing-screen screenshots; suspected exposure → facts only, flag the compliance owner.
- Dispensing outages during open hours are never queued behind routine work — a waiting patient outranks the SLA grid.
- Do not improvise billing or dispensing workarounds; only surface the pharmacy's own documented downtime procedures.
- Counting robots and vendor-locked automation PCs follow the vendor's rules (no ad-hoc patching or agent installs) — schedule changes with the vendor.
- Docs tools vary per tenant — state what you could not verify; a pharmacy with no documented switch/downtime procedure is itself a flag worth raising to the account owner.
