---
name: Trend Root-Cause Mining
description: Someone asks what is making the desk so busy, what is driving ticket volume, or what the top recurring issues are for a period.
category: Reporting & Analytics
tools: [search_tickets, list_boards, search_clients]
---

# Trend Root-Cause Mining

Answer "why is the desk busy?" with evidence: the top recurring volume drivers for the period, each sized, explained, and illustrated with exactly one representative ticket.

## When to use

- "What's making us so busy this week/month?"
- "What are the top recurring issues across the desk?"
- "Volume is up 20% — where is it coming from?"

## Steps

1. Confirm the period (default: last 30 days) and pull the ticket population with split searches per board — and per obvious segment (client, type) when a board would cap; disclose caps.
2. Cluster tickets into drivers by shared signal: same issue type, same client, same asset/service, same alert source, or same request category. Read titles and summaries; do not rely on categories alone if they look inconsistently applied.
3. Rank the top 5–8 drivers by ticket count and estimated time consumed (thread length and time entries as proxies).
4. For each driver give: name, size (count and rough % of period volume), the likely root cause in one or two sentences, and **one representative ticket** as the example — never a full listing.
5. Tag each driver with its fix lane: automation candidate (intent/flow), monitoring retune, client training, project/RCA needed, or vendor issue.
6. Close with the 2–3 drivers most worth acting on and a methodology note (period, boards, clustering basis, caps).

## Guardrails

- One representative example per driver — aggregate everything else. A wall of ticket numbers is a failure mode.
- Exclude junk/NOC boards and auto-closed noise unless the question is specifically about alert noise; if noise dominates real volume, say so as its own finding.
- Distinguish "busy" from "many tickets": a few heavy escalations can outweigh dozens of password resets — weigh by effort, not count alone.
- Root causes are hypotheses read from ticket text; label them as such and recommend verification before investment.
- Do not invent ticket numbers, counts, or percentages; if clustering is ambiguous, present the uncertainty.
