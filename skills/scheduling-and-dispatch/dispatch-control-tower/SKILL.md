---
name: Dispatch Control Tower
description: The dispatcher's live picture in one view — unassigned queue, at-risk tickets, today's scheduled work, and who is free right now.
category: Scheduling & Dispatch
tools: [search_tickets, search_members, list_boards, list_ticket_priorities]
---

# Dispatch Control Tower

One read-only board-state snapshot a dispatcher can act from: what's waiting, what's about to burn, what's already committed today, and who has room to take more.

## When to use

- "Give me the dispatch picture" / "what's the state of the queue right now?"
- Mid-day check-ins after a P1 shuffle: "where do we stand?"
- Covering someone else's dispatch seat and needing situational awareness fast.

## Steps

1. Resolve boards and priorities (`list_boards`, `list_ticket_priorities`); cover the desk's support boards, one search per board per signal — result caps apply per search.
2. **Unassigned.** Open unassigned tickets, sorted priority then age. Flag Critical/High past the desk's aging thresholds at the very top.
3. **At-risk.** Assigned but in trouble: high-priority tickets with no activity in the desk's staleness window, tickets the client has responded to without an answer, and anything the dispatcher has flagged to watch. State the risk reason per ticket.
4. **Today's schedule.** Scheduled entries for today by technician: onsite visits, booked remote sessions, blocked focus work — what's committed and when.
5. **Who's free now.** From the active roster (`search_members`, excluding inactive members and anyone flagged out): current open load (priority-weighted), whether they're inside a scheduled block right now, and next free window. Free = light load AND not currently scheduled — show both numbers.
6. Output a four-section briefing in that order (unassigned / at-risk / today's schedule / free now), each a compact table, with a two-line headline on top ("2 Critical unassigned past threshold; 3 techs free after 2pm"). Keep it skimmable in under a minute.

## Guardrails

- Read-only: this skill reports; it never assigns, reschedules, or changes tickets. Hand off to Dispatch & Workload / Workload-Balancing Assignment for actions.
- Show counts and reasons, not just conclusions — the dispatcher must be able to sanity-check "who's free" against the numbers.
- Result-cap honesty: if any search may have capped, mark that section as possibly incomplete rather than presenting it as the full picture.
- "Free" respects scheduled blocks and priority-weighted load, not raw ticket count.
- Exclude inactive members from the roster view; note members whose status (PTO, out sick) the dispatcher has stated.
- Times in the desk's timezone, stated once at the top.
