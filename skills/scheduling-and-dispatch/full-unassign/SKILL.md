---
name: Full Unassign
description: "Fully unassign <member>" in one command — clear the ticket owner AND remove the member's schedule/resource entry so nothing dangling stays on their calendar, with one confirmation before any write. Answers the commonly requested "get this off my plate completely" workflow; runs manually on demand.
category: Scheduling & Dispatch
tools: [update_ticket, update_schedule_entry, search_members]
---

# Full Unassign

"Unassign me" usually means two things at once: take my name off the ticket *and* clear the schedule entry that still has me booked. Doing only one leaves a ghost — an owner with no booking, or a booking with no owner. This does both, together, on one confirmation.

## When to use

- "Fully unassign <user> from this ticket"
- "Take me off this one — owner and my calendar entry"
- "Remove <user> completely, they're out today"
- Reassigning work away from someone who's off/PTO and shouldn't stay on the schedule

## Steps

1. Resolve the member if named (search_members) and identify the ticket. Read the current state: who owns it and whether that member has a schedule/resource entry on it.
2. Show the member exactly what will change, in one summary: "Will clear owner (<user>) on ticket #<n> AND remove <user>'s schedule entry for <date/time>." If the member owns the ticket but has no schedule entry (or vice-versa), say so — only act on what actually exists.
3. Wait for one explicit confirmation covering both writes.
4. On confirm, perform both: update_ticket to clear the owner, and update_schedule_entry to remove the member's resource/schedule record. If reassigning to someone else was requested, set the new owner instead of leaving it blank.
5. Report both results together: "Cleared owner and removed schedule entry for <user> on #<n>." If either write fails, say which succeeded and which didn't — never imply both when only one landed.

## Guardrails

- Confirm before writing. Both changes wait on one explicit go-ahead; this touches ownership and the calendar, so no silent writes.
- Act only on what exists — don't attempt to remove a schedule entry that isn't there, and don't clear an owner the member didn't ask to clear.
- If the ticket would be left with no owner and it's active/at-risk, flag that in the confirmation so the member can reassign rather than orphan it.
- Report partial success honestly if one of the two writes fails.
- No fabrication of schedule entries or owner history.
- Member-invoked only; no unattended variant (this is a deliberate two-part write that should stay human-confirmed).
