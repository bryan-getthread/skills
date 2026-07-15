---
name: QBR & SBR Prep
description: Prepare an internal brief before a quarterly or strategic business review with a client — volume trends, recurring issues, high-effort work, sentiment, opportunities, a suggested agenda, and the questions the client is likely to ask.
category: Account Management
tools: [search_tickets, search_clients, search_contacts, liongard_cyber_risk_dashboard, liongard_metric, search_ninjaone_devices]
---

# QBR & SBR Prep

Build the internal briefing an account manager walks into a QBR or SBR with: what happened, what it means, what to propose, and what the client will push on. Tuned to surface what the client will not raise themselves.

## When to use

- "I have a QBR with <client> today/this week — prep me."
- "Build my SBR talking points for <client>."
- "What should I bring to <client>'s quarterly review?"
- A 30-day onboarding-readiness check for a recently signed client (run the same brief over the shorter window).

## Steps

1. Confirm the client with search_clients and the review period. Default to the last full quarter; for a new-client check, use the first 30 days.
2. Pull the period's tickets with search_tickets, and the prior period's for comparison. Exclude merged and auto-resolved noise from counts.
3. **Volume trends.** Opened vs closed, this period vs last, and the direction of travel. If any search hit a result cap, say so and report an order-of-magnitude figure instead of a false-precise count.
4. **Top recurring issues.** Rank the top four or five by impact (effort and disruption, not raw count). Give exactly one representative example per pattern, described in plain language — no ticket IDs.
5. **High-effort work.** The top five long-running or high-effort tickets, one line each: what it was, why it was hard, where it stands.
6. **Sentiment trend.** Direction over the period and what drove any low points. Quote themes, not individuals.
7. **Opportunities.** Projects, infrastructure updates, or training that would cut volume or improve experience — each tied to evidence from steps 4–6.
8. If Liongard is enabled for this tenant, add a **security posture** section from liongard_cyber_risk_dashboard and relevant liongard_metric reads: headline risks and changes since last review. Skip silently if not connected.
9. If asked for a **hardware-refresh forecast** and an RMM is connected, use search_ninjaone_devices to summarize devices at or past a four-to-five-year cycle by count and type — no serial-level detail in the brief.
10. Close with a **suggested agenda** (five or six items ordered by what matters to the client) and **anticipated client questions** — the three or four things they are most likely to ask, each with a one-line suggested answer.

Output a section-headed internal brief in the order above. Keep it tight enough to skim in five minutes.

## Guardrails

- This is internal prep, not a client deliverable. Say so at the top of the output. If the AM wants a client-facing version, that is a separate pass with ticket IDs and internal commentary stripped.
- Exactly one representative example per recurring pattern; aggregate the rest.
- Never present a capped search result as a complete count.
- If the data points at an individual technician's performance, frame it as a process gap. Employee-performance evaluation belongs with the manager — redirect there; do not score people.
- Do not invent metrics, links, or ticket numbers. If a section has no data (e.g. no sentiment scores in the period), say "no data" rather than filling it.
- Liongard and RMM sections degrade silently when those integrations are absent — never block the brief on them.

## Consolidates

QBR prep, SBR prep, quarterly summary report, Liongard-posture QBR variant, hardware-refresh forecast variant, and the 30-day new-client onboarding-readiness check.
