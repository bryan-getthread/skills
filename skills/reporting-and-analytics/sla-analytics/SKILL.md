---
name: SLA Analytics
description: Someone asks how the desk is doing against SLA — first-response and resolution performance versus targets, and which tickets breached and why.
category: Reporting & Analytics
tools: [search_tickets, list_boards, list_ticket_priorities, list_ticket_statuses]
---

# SLA Analytics

Report first-response and resolution performance against the desk's targets for a period, with a breach list that explains causes — so the follow-up is fixing the driver, not just staring at a percentage.

## When to use

- "How did we do against SLA last month?" / "what's our first-response time?"
- "Show me every SLA breach this week and why it happened."
- A lead investigating whether a specific board, client, or priority tier is slipping.

## Steps

1. Confirm the period, boards in scope (list_boards), and the SLA targets per priority. If targets are not configured or stated, ask for them — do not assume industry defaults silently.
2. Run split searches per priority tier per board (result-cap honesty: many small searches, not one big one; disclose caps).
3. Report per tier: tickets in scope, first-response attainment, resolution attainment, and the trend versus the prior period if available.
4. Build the **breach list**: for each breached ticket, one line — ticket, client, tier, which clock breached, by how much, and the apparent cause read from the thread (no owner assigned, waiting-on-client misuse, after-hours arrival, vendor hold, alert storm).
5. Group breach causes and rank them: "60% of breaches trace to unassigned overnight intake" is the actionable output.
6. Recommend 2–3 fixes matched to the top causes, and close with a methodology note (targets used, boards, searches, caps, exclusions).

## Guardrails

- Exclude junk/NOC boards and auto-closed statuses; a monitoring auto-close is neither a response nor a resolution.
- Distinguish paused clocks (waiting-on-client, scheduled) from running ones; if the data cannot distinguish them, say the attainment number is approximate and why.
- Never present a capped search as a complete breach census — disclose and estimate.
- Do not name-and-shame individual techs in the breach list; attribute causes to process patterns, and leave person-level follow-up to the manager.
- Do not invent SLA targets, ticket numbers, or breach timestamps.

## Unattended (Flows) variant

- Entire reply is the report, fixed sections: Attainment / Breaches / Causes / Actions / Methodology.
- If targets are unavailable, output only the timing distributions and a line stating targets were not provided — do not guess targets.
