---
name: Supporting Legal Firms
description: Vertical pack for law-firm clients — the DMS and practice stack (iManage, NetDocuments, Clio-class), matter confidentiality and ethical walls, litigation-hold interplay, court-deadline and billable-hour urgency. Load when the client is a law firm or the ticket names a document management system, time-and-billing platform, or e-filing.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Legal Firms

Law firms sell hours and confidentiality. Every minute an attorney can't work is unbilled revenue with a stopwatch on it, and every document the firm holds is privileged until proven otherwise. This pack layers legal-vertical knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework): the DMS-centric stack, why access requests can't be casually granted, how litigation holds constrain routine IT work, and an urgency model calibrated in six-minute increments.

## When to use

- The client is a law firm or legal department, or the ticket names iManage, NetDocuments, Worldox, Clio, PracticePanther, MyCase, Aderant, Elite/3E, ProLaw, Tabs3, or an e-filing portal
- Access requests to matters, workspaces, or another attorney's documents at a legal client
- Any wipe/reimage/mailbox/offboarding action at a legal client (litigation-hold check)
- "Can't file — the deadline is today" tickets

## The stack you'll meet

- **Document management (DMS) is the crown jewels:** iManage and NetDocuments dominate mid/large firms; Worldox in smaller ones. Everything is organized by *matter* (a case/engagement); documents live in matter workspaces with per-matter security. Common failure surface: the Office/Outlook integration add-ins (save-to-DMS, email filing), sync clients, and check-in/check-out state ("my document is locked").
- **Practice management & billing:** Clio, PracticePanther, MyCase at the small end; Aderant, Elite/3E, ProLaw, Tabs3 upmarket. Time capture is money capture — a broken timer or billing client is a revenue incident to the firm.
- **Around them:** e-filing portals (federal CM/ECF, state systems), comparison/redlining tools (Litera-class), PDF/bates tooling, court-calendaring/docketing software (deadline engines — errors here have malpractice implications, so misbehavior is escalated, not shrugged at), legal research (Westlaw/Lexis sign-ins), and e-discovery platforms (usually run by litigation support, not the desk).

## Confidentiality, walls, and access — the regime

- **Matter confidentiality is structural.** DMS permissions encode attorney-client privilege. Never grant, broaden, or "temporarily open" access to a matter, workspace, or mailbox on a requester's say-so — access changes at a law firm route through the firm's designated approver (records/conflicts/managing partner per their process), even when the requester is senior.
- **Ethical walls (conflict screens)** are deliberate access *denials* protecting the firm from disqualification. A "user can't access X" ticket where X is walled is working as designed — confirm against documentation before treating it as a defect, and never punch through a wall to close a ticket.
- **Litigation holds change the physics of IT work.** When a custodian is under hold, routine actions become spoliation risks: no wipes, reimages, mailbox deletions, aggressive retention changes, or device disposals for held custodians. Before destructive actions at a legal client, check hold status — see onboarding-and-access/litigation-hold for the full interplay. When unknown, ask the firm's hold owner and wait.

## The rhythm and what's sacred

- **Billable-hour math:** attorneys bill in 6-minute increments; a 30-attorney firm losing the DMS for an hour loses real, calculable revenue. Firm-wide DMS, email, or billing outages are top severity in business hours.
- **Deadline days:** court filing deadlines (often 11:59 PM local, but some courts close filing at 5 PM) are absolute. A "can't e-file" or "can't finish the brief" ticket on a deadline day outranks its apparent technical size — ask "when is the deadline?" during triage.
- **Month-end billing runs** (prebills, invoicing) make the billing platform sacred in the last and first days of the month.
- **Sacred systems:** the DMS and its indexes, email (attorneys live in Outlook; email *is* the case file feed), the docketing/calendaring system, and time-capture. Backup and journaling on these are never noise.

## Steps

1. Context first: search_tickets for this firm + this app (DMS add-in and sync issues recur with known fixes), and search_itglue / search_hudu for the DMS/billing documentation, vendor support contracts, and the firm's documented access-approval and litigation-hold process.
2. Triage with two vertical questions: "how many attorneys/staff are blocked?" and "is there a filing or court deadline attached?" Deadline-day and firm-wide tickets go to the top with immediate dispatch.
3. For access tickets: verify whether the denial is a wall or hold by checking documentation and the firm's approver — never conclude "permissions bug" without ruling out intentional denial. Route the request through the firm's approver and record the approval in the ticket before any change.
4. For technical tickets, run the LOB framework with legal splits: DMS issues divide into client/add-in (one user), sync/session, and server/index (everyone); billing issues get flagged to the firm's billing coordinator when data entry could be affected. E-filing failures: verify the court portal's own status before debugging locally, and say plainly when the outage is the court's — the firm may need to invoke the court's technical-failure procedure (their call, not yours).
5. Vendor territory (DMS index rebuilds, database repair, billing-schema fixes) → complete vendor-escalation package per the framework, with the firm's support-contract identifiers, case number logged, and follow-up cadence set — with the deadline noted in the case if one exists.
6. Note in plain text: app/version, scope, deadline context, error verbatim, approvals obtained for any access change, branch, vendor case, verification by the user performing the real workflow (open/save a matter document, run the timer, file a test per portal procedure).

## Guardrails

- Never grant or broaden access to matters, workspaces, mailboxes, or files at a legal client without the firm's documented approver — seniority of the requester is not approval. Never bypass an ethical wall.
- Before any wipe, reimage, mailbox action, retention change, or disposal: litigation-hold check (cross-ref onboarding-and-access/litigation-hold). Unknown hold status = stop and ask the hold owner.
- Client documents and their contents stay out of tickets — describe by matter/document ID behavior, never paste privileged content or screenshots of documents.
- Docketing/calendaring anomalies are escalated to the firm immediately, not quietly worked — missed-deadline risk is a malpractice matter.
- No legal advice, including about privilege, spoliation, or court procedure — flag the firm's compliance/records owner; "flag to the responsible owner" is always the move.
- Never operate on the DMS or billing database outside vendor procedure; credentials stay in the documentation system, referenced by location only.
- Docs tools vary per tenant — state what you could not verify; a legal client with no documented hold process or access approver is itself a flag worth raising.
