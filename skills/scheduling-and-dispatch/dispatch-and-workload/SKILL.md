---
name: Dispatch & Workload
description: A dispatcher wants the full queue picture — unassigned tickets by priority and age, open work per technician, lightest-load assignment proposals, and a daily audit of what got assigned where.
category: Scheduling & Dispatch
tools: [search_tickets, search_members, list_boards, list_ticket_priorities, update_ticket, schedule_ticket]
---

# Dispatch & Workload

The dispatcher's core workflow: see the unassigned queue, see who is carrying what, and distribute new work to the lightest load — with the numbers shown so a human can sanity-check every proposal.

## When to use

- "Show me the unassigned queue" / "what's sitting unassigned right now?"
- "Who's least busy?" / "assign these to whoever has capacity"
- "Give me the morning dispatch view" or "workload by tech"
- "Audit today's assignments — did anything get dumped on one person?"

## Steps

1. Call `list_boards` and `list_ticket_priorities` to resolve the boards and priority ids in play. Scan the support boards the dispatcher works — not internal/admin boards unless asked.
2. **Unassigned queue.** `search_tickets` for open, unassigned tickets per board (one search per board — result caps apply per search). Sort by priority, then age. Flag any Critical/High ticket that has sat unassigned past the desk's threshold (default: Critical > 30 min, High > 2 h — use the desk's own thresholds if the dispatcher states them).
3. **Load table.** `search_members` for the active roster, then `search_tickets` for open tickets per technician. Build a per-tech table: open count broken down by priority, plus scheduled entries for today. Exclude inactive members and anyone the dispatcher has flagged as out.
4. **Complexity, not just count.** Weight the load view: a tech with 4 Critical tickets is not "lighter" than one with 6 Low tickets. Show both raw counts and the priority mix so the dispatcher can judge.
5. **Proposals.** For each unassigned ticket, propose the candidate with the lightest weighted load. Exclude from the candidate pool: the ticket's requester (if a member), inactive techs, and anyone excluded by a client-specific routing rule or pinned exclusion the desk has stated. If two candidates are effectively tied, present both and ask — do not pick arbitrarily.
6. On confirmation, apply assignments with `update_ticket` (and `schedule_ticket` if the dispatcher wants the work on the calendar).
7. **Daily assignment audit** (when asked): pull tickets assigned today, group by assignee, and flag skew — one tech receiving a disproportionate share, or high-priority work landing on already-loaded techs.
8. Output one briefing: unassigned queue table (priority, age, client, title), per-tech load table, proposed assignments with the reason ("lightest weighted load: 3 open, none Critical"), and any threshold breaches at the top.

## Guardrails

- Propose, then confirm. Only assign without confirmation if the team has explicitly opted into auto-assign for this workflow.
- Always show the counts behind a recommendation so the dispatcher can sanity-check; never output just "assign to <tech>".
- Never include the requester or inactive members in a candidate pool.
- Respect client-specific routing rules ("<client> tickets go to the dedicated team") and pinned exclusions over pure load math — say when a rule overrode the load answer.
- If a search hits its result cap, say the view may be incomplete rather than presenting counts as exact.
- Respect scheduled and in-progress work when judging capacity — an empty ticket count with a full calendar is not capacity.

## Consolidates

Load-balanced dispatch, technician workload summary, unassigned-queue monitors, and daily assignment audit skills.
