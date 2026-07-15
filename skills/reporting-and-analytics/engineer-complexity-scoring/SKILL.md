---
name: Engineer Complexity Scoring
description: Someone asks whether workload is fair across engineers, or to score each engineer's open tickets by complexity — because open counts alone don't show who is actually loaded.
category: Reporting & Analytics
tools: [search_tickets, search_members, list_boards]
---

# Engineer Complexity Scoring

Score each engineer's open threads by complexity — not just count them — so workload-fairness and rebalancing decisions rest on effort, not raw ticket totals.

## When to use

- "Is the workload balanced across the team right now?"
- "Score everyone's open tickets by complexity" / "who is actually the busiest?"
- Before rebalancing or assigning a big new ticket: "who has capacity?"

## Steps

1. Pull open tickets per engineer with search_tickets, one search per engineer (and per board if volume may cap); disclose any caps.
2. Score each open thread on visible complexity signals, for example on a 1–5 scale:
   - Thread length and number of back-and-forth cycles.
   - Age and number of distinct troubleshooting attempts in the notes.
   - Escalation markers, vendor involvement, or multi-party coordination.
   - Priority and scope (single user versus site-wide).
3. Produce a per-engineer table: open count, summed complexity score, average complexity, and the 1–2 heaviest threads each.
4. Compare load role-aware: an escalation engineer is expected to hold fewer, heavier threads; a dispatcher's assigned items are pass-throughs. State the lens used.
5. Recommend at most 2–3 concrete rebalancing moves (which thread, from whom, to whom, why) — as suggestions, not actions.

## Guardrails

- Never conclude anything about fairness or effort from open counts alone; the score is the point of this skill.
- Scores are heuristic estimates from ticket text — present them as such, never as measured effort or billable value.
- Do not reassign anything; recommendations only, for a dispatcher or manager to act on.
- Exclude junk/NOC boards and parked/scheduled statuses that do not represent active load.
- Aggregate in the readout; name specific tickets only as the "heaviest thread" examples.
- Include a methodology note: scoring rubric used, searches run, caps hit.
