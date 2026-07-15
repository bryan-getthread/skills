---
name: Escalation Rate Report
description: A manager asks what percentage of tickets escalate, where they go, and why — including which escalations look avoidable and should become training or documentation targets.
category: Reporting & Analytics
tools: [search_tickets, search_members, list_boards, search_knowledge_base]
---

# Escalation Rate Report

Measure how much of the desk's work escalates beyond first line, map where it lands, classify why, and separate the necessary escalations from the avoidable ones — because the avoidable slice is a ranked training and documentation to-do list in disguise.

## When to use

- "What's our escalation rate?" / "how much lands on tier 2/3?"
- Senior engineers complain they are drowning in things tier 1 should handle.
- Planning training or KB investment and wanting data on where first line gives up.

## Steps

1. Confirm the period (default: last full quarter) and how this desk marks escalation — a status, a board move, a tier reassignment, or a type flag (list_boards, list_ticket_statuses via the boards pass). State the detection signal in the output.
2. Run split searches per board: total resolved tickets, and tickets showing the escalation signal. Compute the headline escalation rate with prior-period comparison where available. Record caps.
3. **Where** — break escalations down by destination: which tier, team, or named escalation path receives them, and which categories dominate each path.
4. **Why** — sample escalated tickets per top category and classify the driver: genuinely complex, permissions/access the first line lacks, missing documentation, missing skill, client demanded a senior, or premature escalation. Read the sample, don't guess from titles.
5. **Avoidability pass** — for the drivers that are fixable (missing docs, missing skill, premature), estimate the share of escalations they represent and check search_knowledge_base for whether documentation already exists for the top repeat offenders — an escalation that happens despite a good KB article is a training gap, not a documentation gap.
6. Output: headline rate and trend, destination breakdown, driver classification table with the avoidable share clearly separated from the necessary share, top 3 concrete fixes (each tied to a category and a driver, e.g. "a runbook for <category> would address ~N escalations/month — estimate"), and a methodology note — detection signal, sample sizes, searches run, caps hit.

## Guardrails

- Escalating is often the *right* call — frame the necessary share positively, and never treat a low escalation rate as automatically good (see the FCR anti-pattern: it can mean people sit on things they should hand off).
- Never name-and-shame individual techs for escalating; avoidable-escalation patterns are training targets by category, not by person, and never rank techs on escalation counts alone — tier and ticket mix dominate that number.
- Avoidability estimates are labeled estimates, from a stated sample size — never presented as measured totals.
- Never present capped searches as exact totals; disclose caps.
- Aggregate, not enumerate — name a specific ticket only as an illustrative example of a driver class.
- If the desk has no consistent escalation marker, say so first and report what is measurable (board moves, tier reassignment) with the caveat that the true rate is likely higher.
