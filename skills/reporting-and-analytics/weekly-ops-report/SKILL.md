---
name: Weekly Ops Report
description: A service manager wants the weekly service-desk report — team volume, closures, sentiment, aging, and anything anomalous versus the prior week.
category: Reporting & Analytics
tools: [search_tickets, search_members, list_boards, list_ticket_statuses]
---

# Weekly Ops Report

Produce the weekly operations readout for the service desk: what came in, what got closed, how clients felt, what is aging, and what looked unusual — a fast stats pass first, then targeted deep dives only where something moved.

## When to use

- "Run the weekly ops report" / "how did the desk do last week?"
- A recurring Monday-morning team performance summary for a lead or director.
- "Compare last week to the week before — anything off?"

## Steps

1. Confirm the period (default: the last full week) and the boards in scope via list_boards. Exclude junk/NOC and internal-noise boards up front.
2. **Fast stats pass** — run split searches per signal per board (opened, closed, still open, aged past threshold, negative sentiment) rather than one giant query. Record which searches hit a result cap.
3. Report the headline numbers versus the prior week: volume in, closures, net queue change, aging count, and sentiment trend.
4. **Anomaly scan** — flag anything that moved sharply week over week: a volume spike on one board or client, a closure-rate drop, a sentiment dip, a growing aging bucket.
5. **Targeted deep dives** — for each flagged anomaly only, pull a narrow follow-up search and explain the likely driver in one or two sentences (e.g., an alert storm, one large client incident, a tech out sick per the queue pattern).
6. End with 2–3 recommended actions and a methodology note: period, boards included/excluded, searches run, and any result caps hit.

## Guardrails

- Aggregate rather than enumerate — patterns and counts, not ticket lists; name a specific ticket only when it explains an anomaly.
- Never present a capped search result as an exact total; disclose caps and give order-of-magnitude estimates instead.
- Exclude auto-closed statuses from closure credit so automation noise does not inflate the numbers.
- Per-tech numbers in this report are context, not rankings — benchmark role-aware and never flag a person on volume alone.
- Do not fabricate a prior-week baseline; if last week's data is unavailable, report this week standalone and say so.

## Running this unattended

> **Flows cannot schedule or time-trigger this.** Thread Flows fire on ticket *events* and conditions only — there is no schedule, cron, ticket-age, or elapsed-time trigger. This is a cadence/sweep skill, so run it **manually** on demand, or from an external scheduler that invokes Super Magic. A Flow can only reach it via **Run Skill** on a qualifying ticket event, never "every morning" or "after N hours". The output discipline below applies whenever it runs unattended.

- Your entire reply is the report, posted verbatim — no narration or questions.
- Use fixed sections (Headlines / Anomalies / Actions / Methodology) so downstream consumers can parse it.
- If a search fails or returns nothing, state "no data for <section>" rather than guessing.
