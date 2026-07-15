---
name: Change Request Intake
description: A change ask arrived as prose ("can we upgrade the firewall this weekend?") — normalize it into a structured change request (what/why/scope/when/rollback/risk) and route it to the right approval track before anyone touches anything.
category: Change & Problem Management
tools: [search_tickets, search_knowledge_base, create_ticket, update_ticket, add_ticket_note, list_boards, list_ticket_statuses]
---

# Change Request Intake

Turn a loose change ask — an email, a chat message, a line in a ticket — into a change request structured enough to assess, approve, and later audit. Intake normalizes; it does not approve, schedule, or execute.

## When to use

- "Client wants us to migrate their file server — write this up as a change."
- A ticket contains a change ask buried in an incident thread ("while you're in there, can you also...").
- Normalizing a batch of informal change requests before the weekly change review.
- A tech is about to make a change and there's no change record yet.

## Steps

1. Extract the ask from the source (ticket thread, forwarded email, verbal summary the requester gives you). Identify the requester and, where the change is client-originated, whether they have authority to request it — flag if unclear, don't assume.
2. Normalize into the six intake fields. Write only what the source supports; mark gaps as OPEN rather than inventing content:
   - **What** — the change, specific enough to execute from (system, component, from-state → to-state).
   - **Why** — business justification in one or two sentences.
   - **Scope** — systems, sites, clients, and users affected; explicitly name what is OUT of scope.
   - **When** — requested window, or "no window proposed" if none was given.
   - **Rollback** — the reversal path the requester or tech described. If none was described, write "rollback plan: OPEN — required before approval." Never draft one on their behalf.
   - **Risk** — what could go wrong and who feels it, as an initial sketch (the full scoring is change-risk-assessment's job).
3. Check for an existing change record covering the same ask (search_tickets on the change board) — extend it rather than creating a duplicate.
4. Create or update the change ticket on the desk's change board (list_boards; create_ticket / update_ticket) with the structured fields as a plain-text body. Title pattern: "CHANGE: <system/component> — <one-line what>".
5. Route to the approval track by an initial read of risk and precedent:
   - Matches a documented pre-approved standard change (search_knowledge_base for the desk's standard-change list) → note it as a standard-change candidate; it still gets confirmed in change-risk-assessment.
   - Anything else → normal track: hand off to change-request-prerequisites (compliance-and-audit) for the pre-approval completeness gate, then change-risk-assessment for classification.
   - The requester says it must happen NOW to restore or protect service → flag as an emergency-change candidate and point at emergency-change-handling; do not let urgency skip the record.
6. Output: the change ticket reference, the six fields with any OPEN gaps listed, and the recommended track with one line of reasoning.

## Guardrails

- Intake never approves and never schedules — it produces the record and the routing recommendation. Silence from an approver is not approval.
- Never fabricate rollback plans, windows, or justifications to make the record look complete; an honest OPEN beats a hollow field.
- Changes smuggled inside incident tickets ("while you're in there") get their own change record — don't let scope creep ride an incident's approval state.
- Check freeze calendars at intake (see maintenance-freeze-windows); a request landing inside a freeze gets flagged immediately, not discovered at scheduling.
- Plain-text ticket bodies and notes — these records sync to the PSA.
