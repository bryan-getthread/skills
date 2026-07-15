---
name: Escalation Completeness Gate
description: Review tickets entering an "Escalation Requested" status against the escalation checklist and bounce incomplete ones back to the requesting technician with specific, itemized feedback.
category: Escalation
tools: [search_tickets, update_ticket, add_ticket_note, list_ticket_statuses]
---

# Escalation Completeness Gate

Quality gate in front of the escalation queue: grades each escalation request against a fixed checklist, passes complete ones through to the escalation board, and returns incomplete ones to the requesting technician with exactly what is missing. The fix-forward loop always leaves the ticket with the tech — the gate never fills gaps itself.

## When to use

- Embedded in a Flow that fires when a ticket moves to "Escalation Requested" (or the tenant's equivalent status).
- A lead asks to "review the escalation queue for completeness" or "check whether these escalations are ready."
- Senior tiers complain escalations arrive thin and you want the gate run retroactively over the current escalation queue.

## Steps

1. Fetch the ticket's full thread and any internal notes. Resolve the tenant's status names via list_ticket_statuses rather than assuming them.
2. Grade against the checklist — each item pass/fail with the evidence found (or not found):
   - Issue summary that a cold reader can act on
   - Environment: affected <device>/system, versions, scope
   - Impact: who and how many are affected; business urgency
   - At least one troubleshooting step attempted **with its result** (zero steps = automatic fail)
   - Error text or diagnostic evidence, where the issue type produces any
   - What the next tier is being asked to do
   - Correct classification (type, priority) and contact set
3. If every item passes: move the ticket to the escalated status/board via update_ticket and add a one-line plain-text note confirming the gate passed.
4. If any item fails: set the ticket back to the pre-escalation working status, keep it assigned to the requesting technician, and post a plain-text note listing **only the failed items**, each with a specific instruction ("Add the result of the DNS flush you mentioned", "State how many users at <client> are affected"). Never a generic "needs more detail."
5. The tech completes the gaps and re-requests escalation; the gate re-runs on the next status change. Do not cap the loop — but if the same ticket fails 3+ times, add a line suggesting the tech and their lead review it together.

## Guardrails

- The gate gives feedback; it never invents or fills in missing checklist content itself.
- Bounce-backs go to the requesting technician — never reassign to someone else, never leave the ticket in the escalation status when incomplete.
- Feedback must be itemized and specific to this ticket; quote what exists, name what doesn't.
- Do not judge the technical quality of the troubleshooting — only whether it is documented. Judgment calls belong to the senior tier.
- All notes plain text (PSA sync).

## Unattended (Flows) variant

- Trigger: status change to "Escalation Requested."
- Your entire reply is posted verbatim as the internal note — no narration, no preamble, no questions. Output either the pass-confirmation line or the itemized failure list.
- Status moves are the only writes: escalated status on pass, prior working status on fail. Never change assignee, board, priority, or contact.
- If you cannot read the full thread, cannot resolve the tenant's statuses, or the checklist result is ambiguous — do nothing and leave the ticket untouched. When in doubt, do not bounce.
