---
name: New vs Resolved Report
description: Someone wants inflow versus outflow for a period — are we closing tickets as fast as they arrive, per board?
category: Reporting & Analytics
tools: [search_tickets, list_boards, list_ticket_statuses]
---

# New vs Resolved Report

Compare tickets created against tickets resolved over a period, per board, so the desk knows whether it is keeping pace, gaining, or losing ground. The net of the two is the backlog delta; this report explains the flow behind Backlog Trend Report.

## When to use

- "Are we closing as fast as they come in?" / "what's our inflow vs outflow?"
- Weekly or monthly ops review where the balance of the desk is the headline.
- Diagnosing a rising backlog — is it more inflow, or slower outflow?

## Steps

1. Confirm the period and cadence (e.g. weekly buckets), boards in scope (list_boards), and the definitions: created = opened in the bucket; resolved = moved to a resolved/closed status in the bucket. State the resolved-status set.
2. Run split searches per bucket per board for created and for resolved separately (result-cap honesty: many small searches; record caps).
3. Per board and overall, report created, resolved, and the net (resolved − created) each period, plus the ratio (resolved ÷ created). A ratio ≥ 1 means holding or gaining; < 1 means losing ground.
4. Show the running net across the window so the cumulative direction is visible, not just the per-bucket wobble.
5. Interpret: is an adverse trend inflow-driven (rising created) or outflow-driven (falling resolved)? This determines whether the fix is demand-side (deflection, automation) or capacity-side (staffing, process).
6. Output: the per-board created/resolved/net/ratio table, the cumulative net, the inflow-vs-outflow read, and a methodology note (resolved-status set, cadence, boards, period, caps, exclusions).

## Guardrails

- Keep created and resolved on the same bucketing and boards, or the comparison is invalid.
- Bulk auto-closes and mass reclassifications distort resolved counts — flag any bucket where a spike looks like a cleanup rather than real work, and exclude or annotate it.
- The ratio is a flow measure, not a productivity score; do not slice it by tech here or attribute it to individuals.
- Never present capped searches as exact totals — disclose and estimate; caps on the created or resolved side skew the net.
- Do not invent counts for missing buckets — show the gap. For the backlog trajectory itself, cross-reference Backlog Trend Report.
