---
name: Dispatcher KPI Pack
description: A dispatcher (or their lead) wants the dispatcher's own scorecard — time-to-assignment, assignment accuracy via reassignment rate, and unassigned-ticket aging — framed for self-improvement, not blame.
category: Reporting & Analytics
tools: [search_tickets, search_members, list_boards, list_ticket_statuses]
---

# Dispatcher KPI Pack

Build the dispatcher's scorecard for a period: how fast tickets got assigned, how often the first assignment stuck, and what sat unassigned too long — framed as the dispatcher's own improvement instrument, since dispatch quality is mostly invisible until it is measured.

## When to use

- "How am I doing on dispatch?" / "Pull my dispatcher stats for the month."
- A service coordinator preparing for a 1:1 or self-review.
- A lead wants to know whether triage-to-assignment is getting faster or slower.

## Steps

1. Confirm the period (default: last full month) and boards in scope via list_boards; exclude boards the dispatcher does not route. Confirm whose dispatch work is being measured via search_members.
2. **Time-to-assignment** — search_tickets per board for tickets created in the period; measure created → first assignee where the data exposes it. Report median and worst-decile, not just the average (one stuck ticket skews a mean). If first-assignment time is not directly visible, use the best available proxy and say which proxy you used.
3. **Assignment accuracy** — approximate via reassignment: tickets whose assignee changed after initial routing, or that bounced boards. State plainly that this is a proxy (some reassignments are legitimate — vacations, escalations); classify the visible reasons before counting them against accuracy.
4. **Unassigned aging** — search for currently open, unassigned tickets per board, bucketed by age (under 4h / 4–24h / over 24h). This is the live-risk section, not a historical stat.
5. Compare each number to the prior period if available; do not fabricate a baseline.
6. Output: a compact scorecard (metric, this period, prior period, direction), the three oldest unassigned tickets as the immediate to-do, one self-improvement suggestion per weak metric, and a methodology note — period, boards, proxies used, searches run, and any result caps hit.

## Guardrails

- Self-improvement framing throughout: the audience is the dispatcher improving their own craft. Never present this as a disciplinary artifact, and never volume alone — routing 200 tickets badly is not better than routing 80 well.
- Reassignment rate is a proxy for accuracy, not a verdict; always show the legitimate-reassignment caveat in the output.
- Never present a capped search result as an exact count; disclose caps and give order-of-magnitude figures.
- Aggregate rather than enumerate — name specific tickets only in the unassigned-aging to-do list.
- If the desk has no dedicated dispatcher (round-robin or self-assignment), say the scorecard measures the desk's routing process, not a person, and reframe accordingly.
