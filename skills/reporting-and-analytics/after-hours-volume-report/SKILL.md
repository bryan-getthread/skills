---
name: After-Hours Volume Report
description: Someone asks what comes in after hours or on weekends — volume, urgency mix, which clients drive it, staffing implications, and whether the on-call burden is being shared fairly.
category: Reporting & Analytics
tools: [search_tickets, search_clients, search_members, list_boards, list_ticket_priorities]
---

# After-Hours Volume Report

Map the desk's after-hours and weekend ticket patterns: how much arrives, how urgent it really is, who sends it, what it implies for staffing and on-call design, and whether the humans carrying the pager are carrying it evenly.

## When to use

- "What actually comes in after hours?" / "do we need a night shift?"
- Debating on-call compensation, rotation size, or an after-hours surcharge.
- An engineer says the on-call load is unsustainable and someone wants data.

## Steps

1. Confirm the period (default: last 90 days — after-hours patterns need more history than a week), the desk's business-hours definition and timezone(s), and boards in scope (list_boards). Multi-timezone desks: state which timezone defines "after hours" for each client group, or the whole report is noise.
2. Run split searches bucketing ticket creation into business hours / weeknight / weekend, per board. Record caps.
3. **Volume & shape** — after-hours share of total volume, the weekly rhythm (which nights, which weekend slots), and trend versus the prior period if available.
4. **Urgency mix** — within after-hours arrivals, split real urgency (outage/security/VIP, via priority and category) from things that merely arrived at night and could wait for morning. This split is the whole staffing argument: 40 tickets a night that can all wait is a morning-queue problem, not a night-shift problem.
5. **Drivers** — which clients and categories dominate after-hours volume; flag any single client generating an outsized share (that is an agreement/expectations conversation, and possibly an automation target — alert-driven noise at night goes to Alert Noise Assessment).
6. **On-call burden fairness** — where after-hours tickets show a responding tech, tally responses and rough disturbance nights per person on the rotation. Frame as workload distribution and burnout risk, never as productivity ranking.
7. Output: headline after-hours share, the hour-by-day pattern (compact text grid), urgency split, top drivers, burden distribution, 2–3 staffing/policy implications phrased as options with trade-offs (rotation change, morning-triage policy, client conversation), and a methodology note — hours definition, timezone, period, searches run, caps hit.

## Guardrails

- The urgent-vs-can-wait split must be evidence-based (priority, category, resolution pattern), not assumed — misclassifying it in either direction burns either money or an engineer.
- Burden fairness is about protecting people: never volume alone, never present on-call response counts as performance, and role-aware framing if some rotation members cover harder tiers.
- Staffing implications are options with costs, not directives — this report informs the decision in Staffing Model Analysis; cross-reference it for the full modeling.
- Never present capped searches as exact totals; disclose caps — after-hours slices are small and caps distort them badly.
- Aggregate, not enumerate; name a specific ticket only to illustrate a genuine after-hours emergency class.
- If created-time data does not reliably reflect client submission time (e.g., email batching), note it as a known distortion.
