---
name: Weekly Exec Scorecard
description: The owner wants their one-glance weekly — the same handful of revenue-relevant ops numbers every week (billable %, SLA, CSAT, backlog), one trend called out, one decision ask — recurring and numeric, not a narrative.
category: Finance & Billing
tools: [search_tickets, search_clients, search_members, list_boards]
---

# Weekly Exec Scorecard

Produce the owner's fixed weekly scorecard: a small, stable set of revenue-relevant numbers (billable share, SLA performance, CSAT, backlog), each with last week and direction, plus exactly one trend worth attention and one decision ask. This is the recurring numeric instrument; CEO Service Desk Brief (reporting-and-analytics) is the narrative business readout for when the owner wants the story — this is for the owner who wants the same five numbers every Monday.

## When to use

- "Send me my weekly numbers" / a standing Monday scorecard for the owner or GM.
- An owner who explicitly wants numbers, not prose ("just the scorecard").
- Setting up the recurring exec view for the first time (agree the metric set once, then keep it stable).

## Steps

1. Confirm the week (default: last full week) and the metric set. Default five: **billable hours share** (billable / total logged), **SLA response performance** (met / measured, where SLA data exists), **CSAT** (score with N — low N stated plainly per CSAT canon), **backlog** (open tickets, net change), **aged tickets** (open past the desk's threshold). The set is agreed once and held stable week to week — changing metrics silently breaks the trendline; flag any change explicitly in the output.
2. Run split searches per metric per board (list_boards; exclude noise boards consistently every week). Record caps.
3. Build the scorecard: metric / this week / last week / direction arrow / 4-week mini-trend where history exists. No fabricated baselines — a metric without history shows "first measurement".
4. **One trend** — pick the single most decision-relevant movement (not necessarily the biggest number) and explain it in two sentences with its likely driver.
5. **One decision ask** — the one thing that needs the owner this week: a client conversation, a hire trigger, a pricing outlier, an SLA-risk client. Exactly one; a scorecard with five asks is a report. If nothing genuinely needs a decision, say "no decision needed this week" — manufacturing an ask erodes the slot.
6. Output: the scorecard table, the trend paragraph, the decision ask, and a two-line methodology footer — week, boards, definitions held stable, caps hit. Total length: fits on one screen, every week, no exceptions.

## Guardrails

- Stability is the product: same metrics, same definitions, same exclusions every week. Any definition change is announced in the output the week it happens.
- One-glance discipline — one screen; if it needs scrolling, cut commentary, never the methodology footer.
- CSAT rides with its N; a 5.0 from two responses is labeled anecdote even on the exec scorecard — especially there.
- Never present capped searches as exact totals; disclose caps in the footer.
- Numbers are desk-level aggregates — no per-tech figures on the exec scorecard; that routes to Tech Utilization Report (leadership-only) or Tech Performance Review with role-aware framing.
- Do not editorialize beyond the one trend and one ask; the owner asked for numbers.

## Unattended (Flows) variant

- Your entire reply is the scorecard, posted verbatim — no narration, no questions.
- Fixed section order (Scorecard / Trend / Decision / Methodology), plain text safe for email or PSA sync — no emojis; use "up/down/flat" words if arrows may not render.
- A metric whose search fails prints "no data" for the week — never a guess, never last week's number carried forward silently.
- If every core search fails, output a single line stating the scorecard could not be produced and why.
