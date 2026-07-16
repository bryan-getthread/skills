---
name: Temp Access Expiry Reminder
description: Manually sweep open temporary-access tickets, flag the ones nearing their expiry date, and draft a reminder plus a revert task so temporary grants don't quietly become permanent. Answers the commonly requested "make sure temp access actually gets removed" workflow; runs manually on demand (Flows can't schedule this).
category: Onboarding & Access
tools: [search_tickets, add_ticket_note, create_ticket]
---

# Temp Access Expiry Reminder

Temporary access is the classic thing that never gets removed — a contractor, a break-glass grant, a "just for this project" permission that outlives the project. Sweep the open temp-access tickets, catch the ones about to expire (or already past), and tee up the revert.

## When to use

- "Check our temp-access tickets — anything expiring?"
- "Which temporary grants are due to be pulled?"
- "Sweep for temp access that should be revoked by now"
- A weekly/periodic access-hygiene pass (run on demand — see the note below)

## Steps

1. Find the open temp-access tickets with search_tickets — filter on the desk's markers for temporary access (ticket type, tag, subject keywords, or a "temp access" board). If a result cap may have truncated the list, say so.
2. For each, read the stated expiry / end date from the ticket. Because Thread has no ticket-age or elapsed-time trigger, this is a **read of the date written on the ticket**, not an automatic timer.
3. Bucket them: **expired** (end date is past), **expiring soon** (within the member's window, e.g. 7 days), and **no expiry recorded** (a risk in itself — flag it).
4. For the expired and expiring-soon items, draft — as previews — a reminder note for the ticket owner and, if the desk works reverts as tickets/tasks, a **revert task** (create_ticket) describing exactly what access to remove for whom.
5. Present the summary table plus the drafts. Nothing is written until the member confirms; on confirm, add the reminder notes and create the revert tasks.

## Guardrails

- **Recommendation, not action.** This skill flags and drafts; it never revokes access itself and never converts a flag into a completed revert. The actual access removal stays a deliberate human step.
- Confirm before writing any note or creating any revert task.
- Rely on the expiry date recorded on the ticket; if none is recorded, flag "no expiry set" rather than inferring one — never fabricate an end date.
- Result-cap honesty on the sweep — don't imply the list is complete if it was truncated.
- Plain-text notes for PSA compatibility; name the specific access and person in the revert task so it's actionable.
- **Manual only.** Thread Flows fire on ticket events and have no schedule/cron and no time-until-expiry trigger, so this cannot be a recurring flow — run it on demand or from an external scheduler.
