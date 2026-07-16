---
name: Dispatcher Morning Ritual
description: A dispatcher's board-open runbook — morning dispatch report, clear unassigned via workload balancing, SLA-risk check, and yesterday's assignment audit — run when a dispatcher says "open the board", "morning dispatch", or on a scheduled weekday flow.
category: Role Rituals
tools: [search_tickets, search_members, list_boards, update_ticket]
---

# Dispatcher Morning Ritual

The dispatcher's first pass of the day, in a fixed order: see the board, get every ticket owned, protect today's SLA clocks, and learn from yesterday's assignments. Target: 20 minutes, unassigned at zero.

## When to use

- "Run morning dispatch" / "open the board" / "get the queue assigned."
- The coordinator's first login before the team huddle.
- A scheduled weekday-morning flow posting the dispatch picture.

## Steps

1. **Board picture (5 min)** — run `skills/scheduling-and-dispatch/morning-dispatch-report`: overnight arrivals, unassigned count, P1/P2 status, who's in today.
2. **Clear unassigned (8 min)** — run `skills/scheduling-and-dispatch/workload-balancing-assignment` over the unassigned queue until it reads zero. Ambiguous tickets (unclear client, unclear skill need) get assigned to the dispatcher's own triage bucket with a note — ambiguity is not a reason to leave a ticket ownerless.
3. **SLA-risk check (4 min)** — run `skills/qa-and-closure/sla-breach-risk-tiering` for clocks landing today. Confirm every at-risk ticket's assignee is actually in today (cross-check step 1's roster); reassign any that landed on someone who's out.
4. **Yesterday's assignment audit (3 min)** — pull yesterday's assignments and scan for: bounced tickets (reassigned within hours), techs who ended the day overloaded, and any pattern worth a routing tweak (feed persistent ones to `skills/role-rituals/dispatcher-weekly-ritual`).
5. Output the dispatch card: unassigned = 0 (or the exceptions and why), today's SLA-risk list with confirmed owners, and one line of audit learning.

## Time-short version

Steps 2 and 3 only — owned tickets and protected clocks are the job; the report and audit can be skipped with a note.

## Guardrails

- Unassigned-zero by assignment quality, not by dumping: workload-balancing rules apply even under time pressure.
- Never assign to a member who is out/unavailable; verify against today's roster.
- Reassignments and priority changes are writes — in attended mode confirm bulk moves (>5 tickets) before executing.
- Skip-with-note beats fake completion; disclose result caps if queue searches may have truncated.
- The audit is for patterns, not blame — no naming-and-shaming in output that gets pasted to a channel.

## Unattended (Flows) variant

- Your entire reply is the posted dispatch card, verbatim — no narration.
- Read-only unless the flow explicitly enables assignment writes; if enabled, only assign unambiguous tickets and list the rest as "needs human dispatch".
- Print all sections even when empty; if a search fails, post what succeeded plus "data incomplete". When in doubt, do nothing.
