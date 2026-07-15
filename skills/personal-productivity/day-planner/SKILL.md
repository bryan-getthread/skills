---
name: Day Planner
description: Plan a technician's day around their calendar and their ticket queue — "I finish at 5:30, I'm in <timezone>" — producing a realistic time-blocked plan, not just a priority list.
category: Personal Productivity
tools: [search_tickets, search_members]
---

# Day Planner

Turns the queue plus the member's real constraints (meetings, finish time, timezone, lunch) into a time-blocked plan for the day. The difference from a digest: this decides *when* each thing happens and is honest about what won't fit.

## When to use

- "Plan my day — I finish at 5:30 and I'm in <timezone>"
- "I have meetings 10–12, fit my tickets around them"
- "What can I realistically get done today?"
- Re-planning mid-day after a disruption ("the morning got eaten, replan from 1pm")

## Steps

1. Gather constraints. Ask (or take from the message): working hours and timezone, fixed commitments (meetings, onsite visits, lunch), and any hard personal cutoff. If the member's calendar is reachable through a connected calendar integration, offer to read today's events from it; otherwise work from what they tell you — never guess a calendar.
2. Pull their open tickets (search_tickets) plus anything scheduled for today. Classify each candidate task by estimated effort: quick reply (≤10 min), standard work block (30–60 min), or deep work (60+ min). Estimates are yours — label them as estimates.
3. Build the plan against the real clock, in the member's timezone:
   - Anchor fixed commitments first; nothing gets scheduled over them.
   - Put SLA-risk and waiting-client replies in the first available slot — a batch of quick replies early buys quiet for the rest of the day.
   - Give deep-work tickets the longest uninterrupted gap, and say which gap that is.
   - Leave ~20% unallocated as reactive buffer for walk-ins and new urgents — a 100%-booked plan is a fiction; say the buffer is deliberate.
   - Respect energy: nothing cognitively heavy in the last 30 minutes; that slot is for wrap-up notes and time entries.
4. Present the plan as a timeline (times in their timezone, e.g. "8:30–9:00 — reply batch: #123, #456, #789"), each block with its tickets and objective.
5. Be explicit about the cut line: which tickets did NOT make today's plan and where they land instead (tomorrow / needs scheduling / delegate candidate). An honest "won't fit" beats a fantasy plan.
6. Offer follow-ups: draft the reply batch, or set schedule entries on the planned tickets (schedule_ticket) — only if the member confirms they want the plan written back.

## Guardrails

- The plan is advisory: do not create or modify schedule entries, tickets, or reminders unless the member explicitly confirms.
- Never invent calendar events, and never assume a timezone — ask if not stated; a plan in the wrong timezone is worse than none.
- Keep the reactive buffer; if the member insists on packing it, note the risk once and comply.
- Effort estimates are estimates — do not present them with false precision, and re-plan willingly when reality diverges.
- If the queue clearly exceeds the day even before planning, say so at the top and lead with the triage decision (what to drop/escalate/reschedule), not a compressed impossible timeline.
