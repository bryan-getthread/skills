---
name: Multi-Tech Scheduler
description: One prompt names a ticket, the techs, and a window — check each tech's availability, then book all of them onto the ticket in a loop, with a single confirmation summary before any write. Answers the commonly requested "schedule the whole crew on this job" workflow; runs manually on demand.
category: Scheduling & Dispatch
tools: [schedule_ticket, technician_availability_check, search_members, update_schedule_entry]
---

# Multi-Tech Scheduler

Some jobs need more than one person on-site at the same time. Instead of scheduling each tech one at a time, name them all in one prompt, check availability across the window, and book the whole crew after a single confirmation.

## When to use

- "Schedule <user A> and <user B> on this ticket for <window>"
- "Book the whole crew for the <client> project Thursday morning"
- "Put three techs on this install next week — same slot"
- Any job requiring simultaneous or coordinated multi-person scheduling

## Steps

1. Parse the ticket, the list of techs, and the target window. Resolve each name to a member with search_members; if any name is ambiguous, resolve it before booking.
2. Check availability for the window across all named techs (technician_availability_check, or reading existing schedule entries). Surface conflicts explicitly: who's free, who's already booked, and the overlap.
3. If not everyone is free, don't silently drop anyone — present the conflict and ask the member how to proceed (adjust the window, swap a tech, or book only the available ones).
4. Show one confirmation summary: "Will book <user A>, <user B>, <user C> onto ticket #<n> for <date/time>." Wait for a single explicit yes.
5. On confirm, loop schedule_ticket once per tech for the same ticket and window. Track each result.
6. Report back one summary: who was booked, who wasn't, and why. If any single booking failed, say which — never report the crew as scheduled when one slot didn't take.

## Guardrails

- Confirm before writing. All bookings wait on one explicit go-ahead over the summary.
- Never book a tech over a hard conflict without surfacing it first; double-booking is worse than asking.
- Report partial success honestly — list exactly who got scheduled and who didn't.
- Resolve every named tech to a real member before booking; don't guess between two people with similar names.
- No fabrication of availability — if the availability check is inconclusive, say so rather than asserting someone is free.
- Member-invoked only; no unattended variant (Flows can't schedule tickets or run this loop).
