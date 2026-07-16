---
name: Capacity vs Demand Report
description: Someone wants a quick read on ticket demand against available tech capacity for the week — are we over- or under-subscribed right now?
category: Reporting & Analytics
tools: [search_tickets, search_members, list_boards, list_ticket_priorities]
---

# Capacity vs Demand Report

A lightweight weekly read comparing incoming ticket demand against the desk's available technician capacity, so a lead can see at a glance whether the team is over- or under-subscribed. This is the fast recurring view; for full workforce modeling (utilization targets, hiring math, scenario planning) cross-reference Staffing Model Analysis.

## When to use

- "Do we have enough people for the load this week?"
- A quick capacity gut-check before committing to a project or absorbing a new client.
- Weekly ops standup input — demand trend vs the team on the floor.

## Steps

1. Confirm the period (default: the week, optionally trailing 4 weeks for context), boards in scope (list_boards), and the capacity inputs: number of techs available and their rough available hours after meetings/PTO. If capacity inputs are not provided, ask — do not assume a headcount or an hours-per-tech figure.
2. Measure demand: tickets created in the period and, where time-entry data exists, hours logged as a proxy for work volume. Run split searches per board (result-cap honesty; record caps).
3. Express both sides in comparable units — if using hours, demand hours vs available capacity hours; if using counts, tickets vs a stated tickets-per-tech-per-day assumption (state the assumption explicitly).
4. Report the balance: a coverage ratio (capacity ÷ demand), the surplus or gap, and the trend across the trailing weeks if pulled.
5. Flag concentration: demand spikes on particular days, or capacity thin on particular days (PTO clustering), so the fix might be scheduling, not headcount.
6. Output: the demand figure, the capacity figure, the ratio and gap, the weekly trend, day-level concentration flags, and a methodology note (capacity inputs and their source, unit basis and assumptions, boards, period, caps).

## Guardrails

- State every capacity assumption (headcount, hours per tech, tickets-per-tech rate); the whole report is only as honest as these inputs, which come from the requester, not from ticket data.
- Keep demand and capacity in the same units — do not compare ticket counts to hours.
- This is a directional weekly signal, not a workforce plan; do not present it as a hiring recommendation. Route sizing and hiring math to Staffing Model Analysis.
- Capacity and demand are team-level here; do not rank individuals or derive per-person utilization scores.
- Never present capped searches as exact demand — disclose and estimate.
- Do not invent PTO, headcount, or availability the requester did not provide.
