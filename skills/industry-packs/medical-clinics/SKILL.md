---
name: Supporting Medical Clinics
description: Vertical pack for medical-clinic and physician-practice clients — the EMR/EHR stack (eClinicalWorks, Athenahealth-class), e-prescribing and lab interfaces, telehealth, HIPAA/PHI ticket hygiene, and on-call urgency. Load when the client is a medical practice or the ticket names an EMR, e-prescribing, or telehealth platform.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Medical Clinics

A clinic ticket is never just an IT ticket: behind "the EMR is slow" is a waiting room filling up, and behind "the doctor can't log in" may be a provider on call at 2 AM who needs a chart *now*. This pack layers medical-vertical knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework): the EMR ecosystem, the interfaces around it, HIPAA discipline in every artifact, and an urgency model that includes clinical hours and on-call.

## When to use

- The client is a medical clinic, physician practice, urgent care, or specialty group, or the ticket names eClinicalWorks, Athenahealth, NextGen, Veradigm/Allscripts, Kareo/Tebra, Epic-connected community access, or similar
- E-prescribing failures (Surescripts connectivity, EPCS token/authenticator problems), lab-interface issues, telehealth (Doxy.me, Zoom for Healthcare-class) not working
- After-hours ticket from a provider on call
- Any medical-client ticket where patient information could land in a note

## The stack you'll meet

- **EMR/EHR:** eClinicalWorks, Athenahealth, NextGen, Veradigm (Allscripts), Kareo/Tebra are the common independent-practice tier; larger or hospital-affiliated groups may run Epic or Oracle Health (Cerner) via remote/hosted access the MSP does not administer. Cloud EMRs turn into browser/Citrix/bandwidth tickets; on-prem ones into database-server and client-version tickets.
- **The interfaces are the fragile part:** e-prescribing via Surescripts (and EPCS — controlled-substance prescribing with two-factor hardware/soft tokens tied to the individual provider), lab orders/results over HL7-style interfaces to Quest/Labcorp or in-house analyzers, immunization-registry and claims/clearinghouse links. An EMR "working" while its lab interface is silently queueing is a classic — check interface status, not just app-opens.
- **Around the EMR:** telehealth platforms, dictation/speech (Dragon Medical-class, ambient scribes), document scanning into the chart, patient-portal and reminder platforms, exam-room peripherals (signature pads, card scanners, label printers).

## HIPAA and PHI in tickets — non-negotiable

The clinic is a covered entity; the MSP is a business associate under a BAA. That makes ticket hygiene a compliance surface:

- **Never paste PHI.** No patient names paired with clinical details, no screenshots of charts or schedules, no forwarded message bodies containing results. Describe reproduction generically ("opening any patient's meds tab errors") or use a chart/account number alone when a specific record is unavoidable.
- **Minimum necessary** applies to you: collect the least patient-identifying information that still lets the ticket move.
- **Misdirection = flag, don't fix.** Email to the wrong recipient with PHI, a lost/stolen device that touched the EMR, an open share with scans in it: record facts (what, when, scope), notify the practice's designated privacy/compliance owner and your internal escalation path. Do not opine on whether it is a reportable breach — that determination is theirs.

## The rhythm and what's sacred

- **Clinical hours are the SLA.** Morning clinic ramp (7:30–9:00) and the post-lunch block are peak; EMR-down during patient hours means providers on paper and a rescheduling cascade. Whole-clinic EMR outage in clinic hours is always top severity.
- **On-call is real urgency.** A provider on call who cannot reach the EMR or e-prescribe after hours is a patient-care problem, not a morning ticket. After-hours tickets from clinical staff get a clinical-impact question answered *first*: "are you with or expecting a patient now?"
- **Downtime procedures exist — reference them.** Clinics are required to have downtime workflows (paper encounter forms). During an outage, pointing staff to their own downtime procedure is a legitimate, valuable interim step.
- **Sacred:** the EMR and its database/hosted session path, e-prescribing (especially EPCS — providers cannot legally phone in most controlled substances), lab-result flow, and the telehealth stack during scheduled virtual visits. Backup failures on EMR-adjacent servers are never routine.

## Steps

1. Establish context: search_tickets for this client + this app (repeat EMR tickets usually have a known local fix — interface restart, Citrix session reset), and search_itglue / search_hudu for the EMR documentation: hosted vs on-prem, vendor support contract, interface inventory, credential *locations*.
2. Triage on clinical impact: whole-clinic outage in clinic hours or an on-call provider blocked → highest priority, immediate dispatch, and remind staff their downtime procedure exists; single-workstation or cosmetic → normal flow with a workaround stated honestly.
3. Run the LOB framework with medical splits: is it the EMR, the session path to a hosted EMR (Citrix/browser/bandwidth), or an *interface* (e-rx, labs, portal)? Interface problems get checked at the interface-status/queue level. EPCS failures are usually the provider's token/second factor — an identity-proofing and vendor matter, not something the desk re-issues.
4. Branch per the framework: environment-side (network, workstation, printing, scanner drivers, security-agent interference) is the desk's; EMR-internal defects, database issues, and interface-engine faults are vendor territory — build the complete vendor-escalation package and log the case number. For hosted EMRs, check the vendor's status page before deep local diagnosis.
5. Note in plain text, PHI-scrubbed: app and access path, scope, clinical-hours impact, verbatim error, interface status if relevant, branch, vendor case, and verification by the clinical user performing the real workflow (open a chart, send a test e-rx per vendor procedure, receive a lab result).

## Guardrails

- No PHI in any ticket artifact beyond minimum necessary; no chart/schedule screenshots; suspected exposure → facts + flag the compliance owner. Never offer breach-determination or legal advice.
- Never operate on the EMR database or interface engine outside vendor-documented procedure; the desk does not edit HL7 mappings or "fix" queued clinical results — data integrity is patient safety here.
- EPCS credentials and tokens belong to individual providers — never share, transfer, or bypass them, and route re-issuance to the vendor's identity-proofing process.
- After-hours clinical tickets get the clinical-impact question answered before anything else; when in doubt, treat as urgent.
- Credentials live in the documentation system; reference location, never paste.
- Docs tools vary per tenant — state what you could not verify, and flag missing EMR/interface documentation as its own follow-up.
