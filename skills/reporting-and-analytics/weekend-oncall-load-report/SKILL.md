---
name: Weekend On-Call Load Report
description: Someone wants the weekend and holiday ticket load and whether the on-call burden is being shared fairly across the rotation.
category: Reporting & Analytics
tools: [search_tickets, search_members, search_clients, list_boards, list_ticket_priorities]
---

# Weekend On-Call Load Report

Focus specifically on weekend and holiday load: how much comes in, how urgent it is, who drives it, and — critically — whether the people carrying the weekend pager are carrying it evenly. This is the weekend-specific cut; for the full nights-and-weekends picture and staffing argument, cross-reference After-Hours Volume Report.

## When to use

- "What's our weekend load like?" / "is on-call fair across the team?"
- Reviewing weekend on-call comp, rotation size, or a holiday coverage plan.
- An engineer says weekend on-call is unsustainable and wants the data behind it.

## Steps

1. Confirm the period (default: last 90 days — weekend slices are small and need history), the desk's timezone(s), the holiday calendar to include, and boards in scope (list_boards). Multi-timezone desks: state which timezone defines the weekend boundary per client group.
2. Run split searches bucketing creation into Saturday / Sunday / holiday, per board (result-cap honesty; record caps). Weekend buckets are small — caps and small-n distort them badly, so caveat aggressively.
3. **Volume & shape** — weekend/holiday share of total, which slots are hot (Saturday morning vs Sunday night), and trend vs prior period.
4. **Urgency mix** — split genuine urgency (outage/security/VIP by priority and category) from tickets that merely arrived on the weekend and could wait for Monday. This split decides whether weekend coverage is even needed at the current level.
5. **Burden fairness** — where weekend tickets show a responding tech, tally responses and rough disturbed-weekend-days per person on the rotation. Frame as workload distribution and burnout risk — never as productivity ranking.
6. Output: weekend/holiday share, the slot pattern, urgency split, top client/category drivers, per-person burden distribution, 2–3 fairness/coverage options with trade-offs, and a methodology note (weekend/holiday definition, timezone, period, searches, caps).

## Guardrails

- Burden fairness exists to protect people: never volume alone, never presented as performance, and role-aware if some rotation members cover harder tiers or larger client sets.
- The urgent-vs-can-wait split must be evidence-based (priority, category, resolution pattern), not assumed — it is the core of any weekend-coverage decision.
- Weekend/holiday samples are small; disclose caps and small denominators explicitly rather than reporting confident percentages off a handful of tickets.
- Coverage and rotation changes are options with costs, not directives.
- If created-time does not reflect true submission time (email batching), note it as a distortion. For the combined nights+weekends staffing case, cross-reference After-Hours Volume Report.
