---
name: Supporting Schools and Education
description: Vertical pack for K-12 school and district clients — SIS/LMS stack (PowerSchool, Canvas-class), FERPA and student-data hygiene, 1:1 device programs and summer refresh, CIPA filtering, E-Rate awareness, and the school-calendar rhythm. Load when the client is a school/district or the ticket names an SIS, LMS, student devices, or classroom tech.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Schools and Education

Schools run on a calendar nothing else uses: the year begins in August, the fleet is measured in the hundreds per building, most users are minors whose data is federally protected, and the network itself may be bought with regulated federal discounts. This pack layers education knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a K-12 school, district, or private school, or the ticket names PowerSchool, Infinite Campus, Skyward, Canvas, Schoology, Google Classroom, Clever, ClassLink, GoGuardian, Securly, or classroom AV
- Student-device (Chromebook/iPad 1:1) tickets, enrollment, or summer-refresh planning
- Anything touching student accounts, records, rosters, or filtering
- Network/equipment projects at a school (E-Rate awareness)

## The stack you'll meet

- **SIS (Student Information System) is the system of record:** PowerSchool, Infinite Campus, Skyward-class — enrollment, grades, attendance, schedules, guardians. Grade-submission and report-card windows make it periodically sacred; attendance entry makes it daily-critical first period.
- **LMS:** Canvas, Schoology, Google Classroom — where instruction actually happens; an LMS outage is a district-wide "class can't proceed" event during school hours.
- **Identity and rostering:** Google Workspace for Education or Microsoft 365 Education, glued by rostering/SSO platforms (Clever, ClassLink) that provision app access from SIS data. Rostering-sync failures present as "half the third grade can't log into the reading app" — check the sync layer before debugging the app.
- **1:1 device programs:** Chromebooks under Google Admin, iPads under Jamf-class MDM, per-student assignment with asset tracking, loaner pools, and repair pipelines. Volume is the story: build repeatable per-device workflows, not artisanal fixes.
- **Filtering and safety:** CIPA-mandated content filtering (GoGuardian, Securly, Linewize-class) plus classroom-monitoring tools. Filtering is legally required for E-Rate-funded schools — never disable or broadly loosen it as a troubleshooting step; category/site exceptions go through the school's designated approver.
- **Classroom AV:** projectors/flat panels, casting, bell/PA systems, and (in many districts) door-access and camera systems that may or may not be in the MSP's scope — know the boundary.

## Student data: FERPA and friends

- **FERPA** protects student education records; many states add student-data-privacy statutes on top, and districts sign data-privacy agreements with each app vendor. For the desk this means: student records get PHI-grade ticket hygiene — no gradebook/SIS screenshots, no student name + record details in notes, minimum necessary always; describe by behavior ("any student in section X errors"), not by student.
- **Disclosure is the school's call.** Requests to hand student data to a new app, a parent, or a vendor route to the district's designated data-privacy owner — the desk does not export or share student records on a teacher's request alone.
- **Directory/guardian sensitivity:** custody situations make guardian contact data and student location information genuinely dangerous to leak — treat "update the parent contact" tickets as SIS-office work, not desk edits.
- Suspected exposure (mis-shared drive, breached edtech vendor, lost unencrypted device) → facts, containment, flag the district's data-privacy/compliance owner. No legal advice.

## The rhythm: the school calendar is the change calendar

- **August is go-live.** The first day of school is a hard deadline for everything: enrollment loads, rostering sync, device handout, new-staff accounts. Late-July-through-first-week is the vertical's tax season — freeze discretionary changes, staff up for the surge.
- **Summer refresh is the maintenance window.** June–July is when reimaging, re-enrollment, OS upgrades, cart maintenance, infrastructure projects, and device redistribution happen. Plan it in spring; a summer wasted is a year of dragging problems.
- **State testing windows are sacred:** during standardized testing, the network, filtering exceptions for the testing platform, and student devices must be flawless — no changes during testing windows, and pre-flight checks before them.
- **Daily pulse:** first-period attendance (SIS), instructional hours (LMS/devices), and dismissal (bells/PA, parent-communication systems).

## E-Rate awareness

Many schools fund network equipment and internet through **E-Rate** (federal Universal Service discounts). The desk's awareness obligations: E-Rate-funded gear has procurement rules and documentation/retention expectations (asset records, competitive-bidding paper trails, ~10-year record retention); equipment disposal/transfer has rules; and consultants/vendors interact with a formal application cycle (forms and filing windows in winter/spring). The move is never to advise on E-Rate compliance — it is to *recognize* when a project touches E-Rate-funded infrastructure and flag the district's E-Rate coordinator/consultant before purchasing, moving, or disposing of funded equipment.

## Steps

1. Context: search_tickets for this school + system history (rostering-sync and cart-charging tickets recur), and search_itglue / search_hudu for district documentation: SIS/LMS inventory, MDM tenant, rostering platform, filtering policy owner, E-Rate coordinator, testing calendar.
2. Triage on the school clock: SIS down first period, LMS down during instruction, or anything during a testing window → top severity; single-device with loaner available → issue loaner, batch the repair. Ask "is a class blocked right now?"
3. For login/access waves: check the rostering/SSO layer (Clever/ClassLink sync status, SIS feed) before app-level debugging — provisioning is the usual culprit for cohort-shaped failures.
4. For device tickets at 1:1 scale: follow the documented per-device workflow (loaner, asset update, repair queue); lost/stolen student devices get MDM lock/locate per district policy and a flag to the school — student-safety and data angles both.
5. For filtering tickets: legitimate unblock/exception requests route to the district's designated approver with the pedagogical justification captured; never broadly disable filtering, even temporarily, as a diagnostic.
6. Run the LOB framework for app failures (exact versions/tenant, change correlation — vendor updates and Chrome/OS releases break edtech constantly — verbatim error, vendor status pages early for cloud SIS/LMS), and build the vendor-escalation package through the district's support entitlement when it's vendor territory; note grade/report-card deadlines in the case.
7. Note in plain text, student-data-scrubbed: system, cohort scope, school-clock impact, sync-layer findings, approvals for any exception, vendor case, verification by a teacher/student account performing the real workflow.

## Guardrails

- Student records get minimum-necessary hygiene: no SIS/gradebook screenshots, no student names paired with record details in tickets; disclosure decisions belong to the district's data-privacy owner. No legal advice on FERPA — flag the compliance owner.
- Never disable or broadly loosen CIPA content filtering; exceptions are scoped, approved by the district's designated owner, and documented.
- No changes during state-testing windows; pre-flight the testing stack before them.
- Recognize E-Rate-funded equipment before purchase, move, or disposal decisions and flag the district's E-Rate coordinator — procurement and retention rules apply that the desk must not improvise around.
- Guardian/custody data edits route to the SIS office, not the desk.
- August go-live and grade-submission windows carry change freezes on the SIS/rostering path.
- Docs tools vary per tenant — state what you could not verify; a district with no documented filtering approver or testing calendar is itself a flag worth raising.
