---
name: Backlog Trend Report
description: Someone asks whether the open backlog is growing, shrinking, or flat over time — the trajectory, not just today's open count.
category: Reporting & Analytics
tools: [search_tickets, list_boards, list_ticket_statuses, list_ticket_priorities]
---

# Backlog Trend Report

Chart the open-ticket backlog over time so the question "is it getting better or worse?" gets a trajectory, not a snapshot. Turns a single scary open count into a trend with a direction and a slope.

## When to use

- "Is our backlog growing?" / "are we digging out or falling behind?"
- Leadership wants a trend line, not just "we have 340 open tickets right now."
- Justifying a hire, or checking whether a recent process change bent the curve.

## Steps

1. Confirm the window and the sampling cadence (e.g. weekly points over 13 weeks), boards in scope (list_boards), and which statuses count as "open backlog" vs paused (waiting-on-client, scheduled) vs closed. State the status map — it defines the whole chart.
2. Decide the measurement method and state it honestly: point-in-time open counts sampled per period is the clean approach; if the data only supports reconstructing backlog from created/closed dates, say so and note the approximation.
3. Run split searches per period per board to build the series (result-cap honesty: many small searches; record caps). Track total open, and optionally the paused subset separately so a growing "waiting-on-client" pile is not mistaken for real backlog growth.
4. Report the trend: the series as a compact text sparkline/table, the net change over the window, and the slope (growing / flat / shrinking). Segment by board and by priority so a rising critical backlog is not hidden inside a flat total.
5. Add context lines where known: a volume spike, a staffing change, or a process change that aligns with an inflection point — correlation noted as such, not causation asserted.
6. Output: the trend series, direction + slope per segment, notable inflections, and a methodology note (status map, cadence, method, boards, caps, exclusions).

## Guardrails

- State the measurement method up front; a reconstructed backlog and a sampled point-in-time backlog are different numbers and must not be presented interchangeably.
- Separate true open backlog from paused/waiting tickets, or a growing waiting-on-client pile masquerades as a capacity crisis.
- Segment by priority — a flat total can hide a rising critical count.
- Never present capped searches as exact totals; disclose caps, which distort per-period slices badly.
- Note inflections as correlated context, never as proven cause.
- Do not invent counts for periods where data is missing — show the gap.
