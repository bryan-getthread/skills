---
name: Category Mix Report
description: Someone wants the distribution of ticket categories/types for a period and how that mix has shifted over time — what the desk actually spends its time on.
category: Reporting & Analytics
tools: [search_tickets, list_boards, list_ticket_statuses]
---

# Category Mix Report

Show what kinds of tickets the desk is handling — the category/type distribution for a period and how the mix is shifting — so planning, automation, and staffing target the real workload rather than assumptions.

## When to use

- "What are we actually working on?" / "what's our ticket mix?"
- Spotting a rising category worth an automation, a KB article, or a training push.
- Planning inputs — the category shift over quarters informs where to invest.

## Steps

1. Confirm the period, boards in scope (list_boards), and the categorization field(s) in play — type/subType/item, ticketCategory, or a custom taxonomy. State which field defines "category" for this report.
2. Run split searches per category value per board (result-cap honesty: many small searches; record caps). Count uncategorized tickets explicitly — a large blank bucket is itself a finding.
3. Report the distribution: each category's share of volume, ranked, with counts and percentages. Keep a visible "uncategorized" row rather than hiding it.
4. **Shift over time** — compare the current period's mix against a prior period and surface the movers: categories rising or falling in share, not just in raw count (volume growth can lift everything; share change is the real signal).
5. Optionally weight by effort where time-entry data exists — a small-count category that eats hours matters more than its ticket share suggests. Note when effort data is unavailable and the report is count-only.
6. Output: the ranked mix table, the period-over-period movers, the uncategorized share, any effort-weighting caveat, and a methodology note (category field, boards, period, searches, caps).

## Guardrails

- State which field defines category; mixing type, subType, and custom category produces a meaningless blend.
- Always surface the uncategorized/blank share — suppressing it overstates the confidence of the mix.
- Distinguish share change from count change; report both so growth is not mistaken for a mix shift.
- If effort-weighting is used, state the time-entry coverage — partial coverage biases the weighting.
- Never present capped searches as exact totals — disclose and estimate.
- Do not invent categories or reclassify tickets to tidy the chart.
