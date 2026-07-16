---
name: SLA Attainment By Client
description: Someone wants SLA hit-rate broken out per client for a period — which clients we are keeping our promises to and which are quietly slipping.
category: Reporting & Analytics
tools: [search_tickets, search_clients, list_boards, list_ticket_priorities, list_ticket_statuses]
---

# SLA Attainment By Client

Roll SLA performance up by client for a period — first-response and resolution attainment per client, ranked so the account conversations go to the right names. This is the per-client cut of SLA Analytics; use SLA Analytics for the desk-wide breach-cause deep dive.

## When to use

- "Which clients are we missing SLA on?" / "SLA hit-rate by client last month."
- QBR or account-review prep where per-client promise-keeping is the headline.
- A CSM suspects one account is absorbing most of the breaches and wants proof.

## Steps

1. Confirm the period, the SLA targets per priority tier, and boards in scope (list_boards). If targets are not configured or stated, ask — do not assume industry defaults.
2. Resolve the client set (search_clients). Decide the grouping key up front: by end-customer, or rolled to the parent/agreement — state which.
3. Run split searches per client per priority tier (result-cap honesty: many small searches, not one wide one; record every cap hit).
4. Per client report: tickets in scope, first-response attainment %, resolution attainment %, and count of breaches. Show the denominator next to every percentage — 2 of 3 is not "67% and a problem," it is a small-sample caveat.
5. Rank clients by breach count and by attainment gap; separate "chronically slipping" (low % across a real volume) from "one bad ticket" (tiny denominator).
6. Output: a per-client table, the ranked watch-list, and a methodology note (targets used, boards, grouping key, period, searches, caps, exclusions).

## Guardrails

- Suppress or flag clients below a minimum ticket count rather than reporting a headline percentage off 1–2 tickets — small denominators mislead account conversations.
- Exclude junk/NOC boards and auto-closed statuses; a monitoring auto-close is neither a response nor a resolution.
- Distinguish paused clocks (waiting-on-client, scheduled) from running ones; if the data cannot, say the attainment is approximate and why.
- Never present a capped search as a complete count — disclose and estimate.
- Attainment is a promise-keeping metric, not a tech-performance metric; do not attribute a client's breaches to named individuals here.
- Do not invent SLA targets, ticket numbers, or breach timestamps. For breach-cause analysis and fixes, cross-reference SLA Analytics.
