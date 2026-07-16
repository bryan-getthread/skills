---
name: Supporting Staffing Agencies
description: Vertical pack for staffing and recruiting clients — the ATS/CRM and VMS stack (Bullhorn, JobDiver, Avionté, erecruit-class), extreme user churn as temps/contractors on/offboard at scale, timekeeping systems, and payroll-cutoff urgency. Load when the client is a staffing/recruiting firm or the ticket names an ATS/VMS, mass onboarding, or timekeeping.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, schedule_ticket, web_search]
---

# Supporting Staffing Agencies

A staffing agency has two populations: a stable internal core (recruiters, account managers, back office) and a large, constantly rotating field of placed temps and contractors who come and go by the dozen. The defining operational fact is churn at scale — onboarding and offboarding is not an occasional event here, it is the daily throughput of the business — and the pay clock is unforgiving: if hours don't reach payroll by cutoff, workers don't get paid. This pack layers staffing knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a staffing, recruiting, or professional-employer/temp firm, or the ticket names Bullhorn, JobDiver, Avionté, erecruit/Bullhorn Talent, Ceipal, JobAdder, or a VMS (Fieldglass, Beeline, VNDLY-class)
- Bulk or rapid onboarding/offboarding of placed workers ("we placed 40 people starting Monday," "this assignment ended, cut off access")
- Timekeeping/time-capture issues (web timeclocks, mobile time apps, VMS timecards) — especially near a pay cutoff
- Recruiter productivity outages: ATS down, email/dialer/sequencing integrations, resume-parsing failures

## The stack you'll meet

- **ATS/CRM (the recruiting core):** Bullhorn is the dominant platform (often with an Outlook/email integration and a marketplace of add-ons); Avionté, JobDiver, Ceipal, JobAdder are common alternatives. The ATS holds candidates, clients, job orders, placements, and activity. Recruiter email/dialer/sequencing integrations hang off it — many "Bullhorn is broken" tickets are actually the email or add-on integration, so isolate that early.
- **VMS (client-side portals):** large end-clients require the agency to work inside a Vendor Management System (Fieldglass, Beeline, VNDLY) for requisitions, submittals, and timecards. VMS access and VMS-timecard problems are frequently client/VMS-side and only fixable there — recognize the boundary and route accordingly.
- **Timekeeping & pay:** web/mobile timeclocks, VMS timecards, and back-office pay/bill and payroll systems (often a separate provider). Time data flowing correctly and on time is the revenue-and-wages pipeline.
- **Adjacent:** background-check/onboarding-paperwork platforms, job-board and resume-database logins (each with its own account), and the identity tenant that governs the internal core.

## Churn at scale — the operational reality

- **Two very different user populations.** The *internal core* (recruiters/back office) gets full identity discipline — managed accounts, MFA, standard on/offboarding. *Placed workers* often need only lightweight, scoped access (a timekeeping login, a VMS timecard, occasionally an email or a specific app) and their access should be **time-boxed to the assignment**. Don't over-provision a temp with core-employee access.
- **Onboarding is a throughput problem.** Placements arrive in batches with hard start dates. The workable posture: a documented, repeatable bulk-onboarding checklist (accounts, timekeeping enrollment, required app access, MFA where applicable) that can run for one worker or forty, ideally template-driven — treat it as a standard process, not a one-off each time.
- **Offboarding is same-day and easy to forget.** Assignments end constantly, and a placed worker whose access isn't revoked at assignment-end is an open door (and often a licensing cost). Assignment-end should trigger prompt, checklist-complete deprovisioning across timekeeping, VMS, any app access, and identity — record each revocation. Confirm the client's authorization/source of truth for who has ended (usually the ATS placement status or the account manager), and never deprovision on an unverified say-so.

## The rhythm and what's sacred

- **The pay cutoff is the clock.** Timecards must be captured, approved, and transmitted by a weekly (sometimes daily) cutoff, or workers miss pay and clients miss billing. Timekeeping or time-transmission failures approaching a cutoff are top severity — ask "when is your pay cutoff?" during triage.
- **Monday mornings and start dates.** New assignments cluster on Mondays; a bulk-onboarding batch that isn't ready when workers show up stalls billable work — plan onboarding ahead of the start date, use schedule_ticket where a dispatch or window is needed.
- **Sacred systems:** the ATS database and its backups (the whole candidate/client/placement book), the timekeeping-to-payroll pipeline during cutoff, and the internal identity tenant. Backup-failure alerts on the ATS are never routine noise.

## Steps

1. Establish context: search_tickets for this client's history (integration breaks and recurring onboarding patterns), and search_itglue / search_hudu for documentation — ATS platform and integrations, VMS portals in use, timekeeping/payroll systems and the pay cutoff schedule, the bulk on/offboarding checklist, and the client's authorization source for placements ending.
2. For onboarding/offboarding, work from the checklist and the ATS source of truth: batch the work, time-box placed-worker access to the assignment, and record each account created/revoked. Verify assignment-end before deprovisioning — never on unverified say-so.
3. Triage on the pay clock: timekeeping/time-transmission failures near a cutoff, or an ATS-down blocking recruiting firm-wide → top severity; single off-cutoff issue → normal, with an honest workaround.
4. Isolate ATS-vs-integration early: confirm whether the failure is the ATS itself or the email/dialer/sequencing/add-on integration — the fix and the vendor differ.
5. Route VMS-side problems honestly: VMS access and VMS-timecard issues are frequently client/VMS-side — identify the boundary, hand the worker/recruiter the right contact, and note it rather than burning hours.
6. Run the LOB framework for platform failures: exact app/version, change correlation, verbatim error, vendor status pages, vendor-escalation package with pay-cutoff impact stated. Note in plain text, worker-PII-scrubbed: system, on/offboarding scope and revocations, cutoff context, error verbatim, boundary handoffs, vendor case, and verification.

## Guardrails

- Time-box placed-worker access to the assignment and deprovision same-day at assignment-end; a temp with lingering access is an open door and a licensing cost. Verify assignment-end against the ATS/account-manager source of truth before revoking — never on unverified say-so.
- Bulk on/offboarding is a standard, checklist-driven, recorded process — every account created or revoked is logged; partial offboarding is a security gap.
- The pay cutoff is a hard external deadline — timekeeping and time-transmission near cutoff outrank most other work; state cutoff impact in escalations.
- No worker or candidate PII beyond minimum necessary in tickets (no SSNs, no background-check contents); reference by internal candidate/placement ID.
- VMS-side problems are frequently only fixable by the end-client/VMS — route them there rather than absorbing unfixable hours.
- Never operate on the ATS database outside vendor procedure; confirm ATS backups are in scope any time you touch backup config — the candidate/placement book is the business.
- Docs tools vary per tenant — state what you could not verify; a staffing client with no documented onboarding checklist, pay-cutoff schedule, or placement source-of-truth is itself a flag for the account owner.
