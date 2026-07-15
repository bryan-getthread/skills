---
name: Alert Noise Assessment
description: Someone asks which monitoring alerts are noise, how much time alerts are costing, or what to retune — hours spent per alert type per client over a period.
category: Reporting & Analytics
tools: [search_tickets, search_clients, list_boards]
---

# Alert Noise Assessment

Quantify what monitoring noise actually costs: hours spent per alert type per client over a period, which alert classes never produce real work, and specific retuning recommendations.

## When to use

- "Which of our monitoring alerts are just noise?"
- "How much time did we burn on alerts last month, and on what?"
- Before a monitoring-stack retune or an alert-board cleanup project.

## Steps

1. Confirm the period (default: last 30 days) and locate the alert-source boards via list_boards. This skill deliberately *includes* the NOC/alert boards other reports exclude.
2. Pull alert tickets with split searches per alert board per client (or per alert-type keyword) to stay under caps; disclose caps.
3. Classify each alert cluster by outcome:
   - **Actionable** — led to real remediation work.
   - **Self-healed** — condition cleared before anyone acted (offline/reconnected pairs, flapping).
   - **Noise** — closed with no action, duplicate storm, or known-benign condition.
4. Estimate hours consumed per alert type per client from time entries where present, and a conservative per-touch estimate (state it) where not.
5. Rank the worst offenders: alert types where noise % is high AND hours are material. Small noisy alerts that cost nothing rank below rare ones that eat hours.
6. Recommend retunes per offender: raise a threshold, add a dedup window, auto-close self-healing pairs, disable a check on a decommissioned asset, or route to a flow. Give one representative ticket per offender.
7. Close with the projected hours saved per month if the top 3 retunes land, and a methodology note (boards, searches, caps, time-estimate basis).

## Guardrails

- Time figures mixing logged entries with per-touch estimates must say which is which; never present estimated hours as measured.
- "Noise" is an outcome judgment read from tickets — a low-priority alert that once caught a dying disk is not noise; check whether the alert class *ever* fired actionably in the period before recommending disablement, and recommend threshold changes over removal when in doubt.
- Recommendations only — never modify monitoring, auto-close policies, or flows as part of this analysis.
- Disclose result caps; alert volumes routinely exceed caps, so per-type per-client splitting is mandatory, not optional.
- Aggregate per alert type and client; do not enumerate hundreds of alert tickets.
