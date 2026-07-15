---
name: Problem Ticket Creation
description: When an incident recurs past the threshold, create a problem/RCA ticket that links the incidents and documents the current workaround — so the pattern gets a permanent owner instead of serial firefighting.
category: Escalation
tools: [search_tickets, create_ticket, add_ticket_note, update_ticket, list_boards, list_ticket_statuses]
---

# Problem Ticket Creation

ITIL problem management, minus the ceremony: once the same incident has recurred enough times, open one problem ticket that owns the root-cause work, wire every incident to it, and write down the workaround techs are currently using so it stops living in people's heads.

## When to use

- "This keeps happening — open a problem ticket" / "we've fixed this five times this month."
- Embedded in a Flow / periodic sweep that detects an incident signature crossing the recurrence threshold (default: 3+ matching incidents in 30 days; tenant SOP wins if it defines one).
- A recurring-issue review or the Escalation Advisor flagged a repeat pattern needing RCA.

## Steps

1. Establish the incident signature: same symptom class, same client or same infrastructure component. Verify recurrence with search_tickets over the window — match on documented symptoms and error text, not title wording alone. List the matching incidents with dates.
2. Check for an existing problem ticket for this signature (search_tickets on the problem board/type). If one exists, link the new incidents to it instead of creating a duplicate.
3. Create the problem ticket via create_ticket on the agreed board (list_boards): title in the pattern "PROBLEM: <symptom> — <client or component>"; body containing the incident list with references and dates, the recurrence rate, common factors observed across incidents (and differences worth noting), and business impact per occurrence.
4. Document the **current workaround** verbatim from what techs actually did in the incident tickets: steps, how long it holds, and its cost per occurrence. If no consistent workaround exists, state "no reliable workaround" explicitly — that raises the problem's urgency.
5. Link both ways: plain-text note on each incident ticket referencing the problem ticket ("Linked to problem ticket <ref> — apply the documented workaround; root cause tracked there"), and the problem ticket lists every incident.
6. Frame the RCA ask: what needs investigating, what data the next occurrence should capture, and a recommended owner tier. Assign only with confirmation.
7. Output: problem ticket reference, incidents linked, workaround status, and the suggested next step.

## Guardrails

- One problem per signature — search before create; duplicates fragment the RCA.
- Recurrence must be evidenced by the listed incidents; do not pad the count with loosely similar tickets to clear the threshold.
- The workaround is transcribed from documented fixes, never invented; if incident tickets show different fixes worked, record them all rather than picking one.
- Creating a problem ticket does not close or alter the incident tickets' own lifecycles — they still resolve individually.
- Notes and ticket bodies plain text.

## Unattended (Flows) variant

- Trigger: scheduled sweep, or an incident-close event that increments a known signature.
- Deterministic gate: create only when the threshold (3 matching incidents / 30 days, or the tenant's configured values) is met by strict signature match — same client-or-component AND same symptom class. Fuzzy matches do not count toward the threshold.
- If an existing problem ticket for the signature is found: add the linking note to the new incident(s) only. Never create a second problem ticket.
- Writes limited to: one create_ticket (problem) and plain-text linking notes. No assignment, no priority changes, no incident-status changes.
- Your entire reply is the problem-ticket body / note content — no narration. If signature matching is ambiguous or searches hit result caps that make the count unreliable, do nothing.
