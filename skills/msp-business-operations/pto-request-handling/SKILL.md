---
name: PTO Request Handling
description: Work a staff PTO/time-off ticket — check coverage against the schedule for those dates, route the approval to the manager, trigger handoff prep, and update calendars once approved. The human approves; this skill runs the process around the approval.
category: MSP Business Operations
tools: [search_tickets, search_members, update_ticket, add_ticket_note, send_approval, schedule_ticket, update_schedule_entry]
---

# PTO Request Handling

Turns a "requesting PTO <dates>" ticket into a clean process: a factual coverage picture for the manager, an approval routed to the right person, handoff prep triggered before the tech leaves, and schedules updated after the yes. The skill never approves or denies anything — it makes the decision easy and the follow-through automatic.

## When to use

- A ticket arrives on the internal board: "PTO request, <date range>."
- "Can <user> take the 14th–18th off — who else is out?"
- After an approval: "PTO approved, set everything up."
- A manager wants the process standardized so requests stop living in chat messages.

## Steps

1. Read the request: who, which dates, full or partial days. If the dates are ambiguous ("the week of the 14th"), confirm before doing anything else.
2. Build the coverage picture for those dates and post it as a plain-text note for the approver:
   - Who else on the same team/role is already out or scheduled away in that window (check schedules and any PTO tickets already approved for overlapping dates — search_tickets on the internal board).
   - The requester's live workload: open tickets, anything scheduled inside the window (search_tickets, schedule entries), and any client commitments (maintenance windows, onsites) that land in those dates.
   - State it neutrally: "2 of 4 L2s would be out Tue–Wed; <user> has one scheduled onsite on the 16th that would need moving." No recommendation to approve or deny.
3. Route the approval (send_approval) to the requester's manager with the coverage note attached. If the manager is unknown, ask — do not guess an approver.
4. If PTO balance or accrual comes up: that lives in the HR/payroll system, not the desk. Say so plainly and never estimate a balance.
5. On approval:
   - Update the ticket status and note the approval (who, when).
   - Move or flag the scheduled items inside the window (update_schedule_entry / schedule_ticket) so nothing is silently orphaned.
   - Trigger handoff prep: point the requester at the vacation-handoff-pack skill (personal-productivity) to triage their open tickets into transfer/watch and brief the covering tech — for absences of 2+ days this is the step that saves the desk.
   - Update calendars: mark the absence wherever the team plans coverage (if the member has connected Outlook via Zapier, `zapier: Microsoft Outlook "Create Event"` for the out-of-office block; otherwise note it for whoever owns the team calendar).
6. On denial or a request to shift dates: relay the manager's message respectfully and factually, and leave the ticket open for the revised request. The manager's reasoning is theirs to give, not yours to invent.

## Guardrails

- **The manager decides.** Never approve, deny, or signal likelihood ("should be fine") — even when coverage looks trivially fine. Present facts; route the approval.
- Coverage notes describe the schedule, not the person: "2 of 4 L2s out" — never "again?", never patterns of who takes leave, never comparisons between staff members' time off.
- PTO reasons are nobody's business. If a reason appears in the request, do not repeat it in notes or the approval — "PTO request, <dates>" is the whole record.
- Never state or estimate PTO balances; the desk is not the system of record for accruals.
- Internal-board only: staff absence details never appear on client-visible tickets. Client-facing coverage messaging (if a named engineer will be away) is a separate, deliberate note the account owner sends.
- Degradation: no Zapier/Outlook connection → skip the calendar write and say the calendar update is manual.

## Unattended (Flows) variant

- Trigger: new ticket on the internal board matching PTO intent.
- Do steps 1–3 only: parse dates, build the coverage note, send the approval to the mapped manager. If dates cannot be parsed unambiguously or no manager mapping exists, add a note asking for clarification and stop.
- Your entire reply is posted verbatim as the ticket note — plain text, no markdown, no narration, no recommendation language.
- Never act on the approval outcome autonomously; steps 5–6 run attended.
