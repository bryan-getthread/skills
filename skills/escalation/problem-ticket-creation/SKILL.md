---
name: Problem Ticket Creation
description: When an incident recurs past the threshold, create a problem/RCA ticket that links the incidents and documents the current workaround — so the pattern gets a permanent owner instead of serial firefighting.
category: Escalation
tools: [search_tickets, create_ticket, add_ticket_note, update_ticket, list_boards, list_ticket_statuses]
connectors: []
---

# Problem Ticket Creation

**When to use:** "This keeps happening — open a problem ticket" / "we've fixed this five times this month"; a periodic sweep detects an incident signature crossing the recurrence threshold; or a recurring-issue review flagged a repeat pattern needing RCA.

## Prompt

```
ITIL problem management, minus the ceremony: once the same incident has recurred enough,
open one problem ticket that owns the root-cause work, wire every incident to it, and
write down the workaround techs are currently using.

1. Establish the incident signature: same symptom class, same client or same
   infrastructure component. Verify recurrence with search_tickets over the window —
   match on documented symptoms and error text, not title wording alone. List the
   matching incidents with dates. Threshold default: 3+ matching incidents in 30 days;
   tenant SOP wins if it defines one.

2. Check for an existing problem ticket for this signature (search_tickets on the
   problem board/type). If one exists, link the new incidents to it instead of creating
   a duplicate.

3. Create the problem ticket via create_ticket on the agreed board (list_boards): title
   "PROBLEM: <symptom> — <client or component>"; body containing the incident list with
   references and dates, the recurrence rate, common factors across incidents (and
   differences worth noting), and business impact per occurrence.

4. Document the CURRENT WORKAROUND verbatim from what techs actually did in the incident
   tickets: steps, how long it holds, cost per occurrence. If no consistent workaround
   exists, state "no reliable workaround" explicitly — that raises the problem's urgency.

5. Link both ways: plain-text note on each incident ticket referencing the problem
   ticket ("Linked to problem ticket <ref> — apply the documented workaround; root cause
   tracked there"), and the problem ticket lists every incident.

6. Frame the RCA ask: what needs investigating, what data the next occurrence should
   capture, and a recommended owner tier. Assign only with confirmation.

7. Output: problem ticket reference, incidents linked, workaround status, next step.

If running unattended as a Flow Run Skill action (trigger: scheduled sweep or an
incident-close event incrementing a known signature): create only when the threshold is
met by strict signature match — same client-or-component AND same symptom class; fuzzy
matches don't count. If an existing problem ticket is found, add the linking note to the
new incident(s) only — never create a second. Writes limited to one create_ticket
(problem) and plain-text linking notes: no assignment, no priority changes, no
incident-status changes. Your entire reply is the problem-ticket body / note content, no
narration. If signature matching is ambiguous or searches hit caps that make the count
unreliable, do nothing.

Guardrails: one problem per signature — search before create. Recurrence must be
evidenced by the listed incidents; don't pad the count with loosely similar tickets. The
workaround is transcribed from documented fixes, never invented; if different fixes
worked, record them all. Creating a problem ticket does not close or alter the incident
tickets' own lifecycles. Notes and ticket bodies plain text.
```
