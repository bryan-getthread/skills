---
name: Supporting Veterinary Practices
description: Vertical pack for veterinary-clinic and animal-hospital clients — the PIMS stack (Avimark, Cornerstone, ezyVet-class), in-house lab analyzers and imaging, controlled-substance record sensitivity, and boarding-hours coverage where patients live on site 24/7. Load when the client is a vet practice or the ticket names veterinary practice-management, lab, or imaging software.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Veterinary Practices

A veterinary practice looks like a dental office until you notice the patients don't go home at 5 PM — hospitalized and boarded animals mean the building, and some of its systems, run around the clock. The stack centers on a practice-management system wired to in-house lab analyzers and imaging, and while there is no HIPAA for animals, the payment data, the client relationship, and the DEA-regulated controlled-substance records are plenty sensitive. This pack layers veterinary knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a veterinary clinic, animal hospital, emergency-vet, or boarding/daycare operation, or the ticket names Avimark, Cornerstone, ImproMed, ezyVet, Shepherd, Digitail, IDEXX Neo/VetConnect, or veterinary lab/imaging gear
- "The schedule/PIMS won't open," "lab results aren't coming across," "x-rays won't load"
- After-hours tickets from a practice with hospitalized or boarded animals
- Anything touching controlled-substance logs or the practice's payment systems

## The stack you'll meet

- **PIMS (practice information management system):** Avimark and Cornerstone are the long-standing on-prem majors (both in the Covetrus/IDEXX orbit), ImproMed likewise; ezyVet, Shepherd, Digitail, IDEXX Neo are the cloud generation. The PIMS is the practice — schedule, medical records, invoicing, reminders, inventory. On-prem means the classic closet server + workstation clients pattern with client/server version-mismatch as the top repeat failure; cloud means the internet line is the practice.
- **In-house lab analyzers:** IDEXX (Catalyst, ProCyte-class) and Zoetis/Abaxis-class benchtop analyzers interface results directly into the PIMS. The interface (often a small connector service or an IDEXX VetLab Station) is its own failure point: analyzers "working" while results silently stop flowing into charts is the classic ticket — check the interface/queue, not just the analyzer screen. Reference-lab results (IDEXX, Antech) arrive over similar integrations.
- **Imaging:** digital radiography (IDEXX, Sound, Vetel-class) and dental x-ray — same shape as the dental pack: capture software bridged to the PIMS, sensor/plate hardware that is expensive to accuse of death (swap-test first), and an imaging database that may not be inside the PIMS backup — confirm both are covered.
- **Also:** payment terminals (PCI — vendor-documented procedures, never handle card data), client-communication apps (reminders, two-way texting), boarding/daycare add-ons with webcams clients watch, and controlled-substance inventory modules or standalone logs.

## Sensitivity: no HIPAA, still plenty sacred

- **Client and payment data:** pet owners' identities, addresses, and stored payment methods get standard confidentiality hygiene — minimum necessary in tickets, no PIMS screenshots with client lists, no card data ever.
- **Controlled-substance records:** veterinary practices stock DEA-scheduled drugs (ketamine, opioids, euthanasia agents) and are required to keep accurate, auditable dispensing logs — often inside the PIMS inventory module. The desk's rules: treat those records' integrity like financial data (no manual "corrections" to make a count look right — discrepancies are the practice owner's to investigate and document); anything suggesting log tampering, unexplained data loss in inventory modules, or requests to alter historical entries gets flagged to the practice owner/manager, not quietly executed. No regulatory advice — "flag to the compliance owner" is the move.
- **Medical-record integrity:** patient (animal) records are legal records in board-complaint and lawsuit scenarios — the same never-edit-history posture applies: fixes go forward as corrections, not silent rewrites, per the PIMS vendor's documented procedures.

## The rhythm and what's sacred

- **The day starts early and slams at open:** drop-offs and morning appointments make 7:30–9:00 AM the peak-fragility window — a PIMS or lab outage at opening cascades through the whole appointment day. Treat opening-time whole-practice outages as top severity.
- **Patients live on site:** hospitalized animals on treatment sheets, boarders over weekends and holidays. Practices with hospitalization/boarding have staff on site nights, weekends, and holidays — meaning after-hours tickets can be legitimate urgent care ("treatment sheets are in the PIMS and it's down at 10 PM"). Ask the clinical question first: "is a hospitalized patient's care affected right now?" Holiday boarding peaks (Thanksgiving, Christmas, summer) raise the after-hours stakes precisely when vendors are closed — pre-flight before holiday weekends.
- **Emergency/after-hours practices** run 24/7 outright and get the full hospital-grade urgency model at all hours.
- **Sacred systems:** the PIMS and its database/backups (schedule + records + receivables in one basket), the lab-interface flow (a sick patient's bloodwork stuck in a queue is a clinical delay), imaging during procedures (dental and surgical x-rays are intra-operative — a capture failure mid-anesthesia is urgent by definition), and the controlled-substance log's integrity.

## Steps

1. Context: search_tickets for this practice + system history (lab-interface restarts and imaging-bridge re-links recur with known fixes), and search_itglue / search_hudu for documentation: PIMS type/version and server, analyzer and imaging inventory with interface details, vendor support contracts and portal-credential locations, whether the practice hospitalizes/boards (defines the after-hours model).
2. Triage on the practice clock and the clinical question: whole-practice PIMS/lab/imaging failure during patient hours or affecting hospitalized-patient care → top severity, immediate dispatch; capture failure during an anesthetized procedure → drop everything; single-workstation with others working → normal flow with an honest workaround.
3. Localize before diagnosing: PIMS itself, the lab/imaging *interface* (connector service, VetLab Station, bridge), the device (analyzer, sensor, plate reader), or the network path. Interface failures get the vendor-documented service restart/queue check first — most vet-lab integrations have one, and it resolves the majority of "results stopped flowing" tickets.
4. Run the LOB framework for the failing component: exact versions client and server (partial PIMS updates are the classic), change correlation (Windows patches breaking analyzer connector services is a recurring pattern), verbatim error, vendor known-issue search — IDEXX/Covetrus-class vendors maintain real KBs; use them before forums.
5. Vendor territory (PIMS database repair, analyzer firmware/hardware, imaging-database faults) → complete vendor-escalation package with the practice's support identifiers and case number; state the clinical context (hospitalized patients, procedures blocked) in the case — veterinary vendors triage on it.
6. Note in plain text, client-data-scrubbed: system + exact versions, scope, practice-clock and clinical impact, interface findings, error verbatim, branch, vendor case, verification by staff performing the real workflow (run a test result across the interface per vendor procedure, capture an image, open the schedule).

## Guardrails

- Never operate on the PIMS, lab-interface, or imaging databases outside vendor-documented procedure — medical records, receivables, and support standing are all in one basket; the vendor package is the deliverable when the desk's branch ends.
- Controlled-substance records: never alter, "correct," or backfill entries; discrepancies, suspected tampering, or alteration requests get flagged to the practice owner. No regulatory (DEA/state-board) advice — flag to the compliance owner.
- Medical-record edits follow the PIMS vendor's correction procedures — forward-dated corrections, never silent history rewrites.
- After-hours tickets get the clinical question first ("is patient care affected right now?"); practices with hospitalization/boarding are treated as 24/7 operations, and holiday boarding weekends get pre-flight checks.
- Never pronounce an x-ray sensor/plate or analyzer dead without swap-testing — expensive accusations require evidence.
- Confirm imaging and lab databases are inside backup scope whenever touching backup config — the PIMS backup often doesn't cover them.
- Client/payment data: minimum necessary in tickets, no client-list screenshots, no card data ever (PCI: vendor procedures only).
- Docs tools vary per tenant — state what you could not verify; a practice with no documented interface inventory or vendor contracts is itself a flag worth raising.
