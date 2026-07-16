---
name: Supporting Home Health & Hospice
description: Vertical pack for home-health and hospice clients — mobile clinicians documenting in patient homes on tablets/phones, EHR and EVV (electronic visit verification) systems, HIPAA in the field, and the connectivity reality of care delivered outside any managed network. Load when the client is a home-health, hospice, or home-care agency, or the ticket names field clinician devices, EVV, or an agency EHR.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Home Health & Hospice

Home health and hospice invert the usual clinical-IT problem: there is barely an office. The work happens in patients' homes, delivered by clinicians who drive all day with a tablet or phone, documenting visits and verifying them electronically over whatever cellular signal exists at a rural address. The device *is* the workplace, connectivity is never guaranteed, and PHI travels in a car — so the pack is about mobile fleet reliability, offline-tolerant workflows, EVV compliance, and HIPAA outside any wall. It layers on the LOB Application Framework (troubleshooting-playbooks/lob-application-framework) and the mobile-device playbook (troubleshooting-playbooks/mobile-device-mdm).

## When to use

- The client is a home-health, hospice, home-care, or in-home therapy agency, or the ticket names a field EHR/agency platform (Homecare Homebase, MatrixCare, WellSky, Axxess, Netsmart, Alora, KanTime-class), or an EVV system (HHAeXchange, Sandata, Tellus/Netsmart-class)
- A field clinician's tablet/phone can't sync, can't capture a visit, or lost documentation after a no-signal visit
- EVV failures: visits not verifying, GPS/telephony check-in/out not recording, state-aggregator rejections
- Connectivity/coverage complaints from clinicians in the field; lost or stolen field devices

## The stack you'll meet

- **Agency EHR (the field-documentation core):** Homecare Homebase, MatrixCare, WellSky, Axxess, Netsmart, KanTime — scheduling, clinical documentation (OASIS assessments for home health), plan of care, billing, and often a *mobile app* clinicians use at the bedside. The defining behavior is **sync**: clinicians document on the device, sometimes offline, and it must reconcile to the server. Most "I lost my note" tickets are sync/offline-mode problems — understand whether the app supports offline capture and how it re-syncs before assuming data loss.
- **EVV — electronic visit verification:** federally mandated (21st Century Cures Act) for Medicaid personal-care and home-health services. The visit is verified by time, location (GPS), and identity at check-in/out, then aggregated to a state system. EVV failures split into device-side (app, GPS permission, telephony fallback), agency-EHR-to-EVV integration, and state-aggregator rejection — and an unverified visit can mean an unpaid visit, so EVV problems have a billing/compliance edge.
- **Devices:** agency-issued tablets/phones (and some BYOD), managed by MDM, on cellular plans — this is a mobile fleet, not a desktop estate. Rugged use, battery, carrier coverage, and app/OS-update breakage are the recurring tickets.
- **Adjacent:** the small back office (intake, billing, QA/coding), the identity tenant, telephony/on-call for after-hours nursing, and faxing (still heavy in home care for physician orders).

## HIPAA in the field — PHI in a car

Home-health agencies are HIPAA covered entities; the MSP is a business associate — assume a BAA and behave accordingly, with the field twist that PHI lives on mobile devices outside any building.

- **Device controls are the safeguard.** Field devices carrying PHI need encryption, screen-lock/PIN, MDM enrollment, and remote-wipe capability — these are not optional niceties, they are the compliance posture for a device that will eventually be left in a car, a patient's home, or a gas station. Confirm they're in place; flag gaps to the agency's privacy/compliance owner.
- **Lost or stolen device is a potential incident.** A missing field device is not a routine "order a replacement" ticket — capture facts (was it encrypted, MDM-enrolled, PHI-bearing, remotely wipeable), initiate remote lock/wipe per the agency's procedure, and flag the privacy/compliance owner immediately. Suspected exposure is a flag, not a fix — no breach-notification opinions.
- **Minimum necessary in every artifact.** No patient names, addresses, diagnoses, or visit contents in tickets or notes; no screenshots of the EHR/visit documentation. Reference a patient by internal record ID and a visit by visit ID. Home addresses are especially sensitive here — never put a patient address in a ticket.

## The rhythm and what's sacred

- **Care is 7-day and after-hours.** Hospice especially runs 24/7 with on-call nurses; home health documents daily. There is no clean maintenance window that misses everyone — coordinate changes to the EHR, EVV, or telephony around the agency's lowest-visit period and on-call schedule, not a generic "after 5 PM."
- **Sync deadlines and billing.** Visits typically must be documented and synced within tight windows for compliance and billing; a fleet-wide sync outage means a day of unbillable, unverified visits piling up — treat it as high severity.
- **Connectivity is assumed-degraded.** Clinicians work where there is no signal. Solutions must respect offline-first reality: prefer workflows and settings that capture offline and reconcile, rather than assuming always-on connectivity. A "fix" that only works on office Wi-Fi hasn't fixed the clinician's day.
- **Sacred systems:** the agency EHR database and its backups, the EVV-to-state pipeline, and the mobile fleet's manageability (MDM/remote-wipe). Backup-failure alerts on the EHR are never routine noise.

## Steps

1. Establish context: search_tickets for this agency's history (sync and EVV issues recur with known fixes), and search_itglue / search_hudu for documentation — EHR platform and mobile-app/offline behavior, EVV vendor and state aggregator, MDM platform and remote-wipe procedure, carrier/device fleet details, and the privacy/compliance contact.
2. For a possible-data-loss ticket, determine sync/offline state first: is the documentation genuinely lost or is it queued in offline mode awaiting re-sync? Preserve the device state and follow the vendor's re-sync path before assuming loss.
3. For a lost/stolen device, treat it as a potential incident: capture the encryption/MDM/PHI facts, initiate remote lock/wipe per procedure, and flag the compliance owner — not a routine replacement.
4. Triage on the care and billing clock: fleet-wide EHR/EVV/sync outage during visit hours, or on-call telephony down for hospice → top severity and immediate dispatch; single-device issue with a workaround → normal, honestly noted.
5. Run the LOB + mobile framework with home-care splits: EVV failures → device (app/GPS/telephony) vs EHR-to-EVV integration vs state-aggregator rejection; sync failures → app offline state vs connectivity vs server; respect offline-first reality in any fix. Route state-aggregator and carrier-coverage problems to the right owner honestly.
6. Note in plain text, PHI-scrubbed (no patient names or addresses): system, device/fleet scope, sync/EVV domain identified, care-clock and billing impact, error verbatim, incident flag if a device is lost, vendor/aggregator case, and verification by the clinician re-running the real workflow (capture a test visit, confirm it verifies and syncs).

## Guardrails

- Field devices carrying PHI must be encrypted, MDM-enrolled, screen-locked, and remotely wipeable — confirm these controls and flag gaps to the compliance owner; a lost/stolen device is a potential incident (facts + remote wipe + flag), never a routine replacement.
- No PHI in tickets — especially no patient names, home addresses, diagnoses, or EHR/visit screenshots; reference by record/visit ID, minimum necessary.
- Respect offline-first reality: before declaring documentation lost, check offline-queue/sync state and follow the vendor re-sync path; prefer fixes that work where there is no signal, not only on office Wi-Fi.
- EVV problems carry a billing/compliance edge (an unverified visit can be unpaid) — classify device vs integration vs state-aggregator, and route aggregator/state rejections to the right owner; never fabricate or edit visit-verification data.
- No clean everyone-misses window exists in 24/7 home care — coordinate changes around the on-call schedule and lowest-visit period, not a generic after-hours slot.
- Never operate on the EHR or EVV databases outside vendor procedure; confirm EHR backups are in scope any time you touch backup config.
- Suspected PHI exposure → contain, record facts, flag the privacy/compliance owner at once; no legal or breach-notification advice.
- Docs tools vary per tenant — state what you could not verify; an agency with no documented remote-wipe procedure, EVV configuration, or compliance contact is itself a flag for the account owner.
