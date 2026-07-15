---
name: Zero-Touch Opportunity Mining
description: Someone asks where the biggest opportunities are to increase zero-touch resolution, or which ticket patterns should become intents or flows.
category: Reporting & Analytics
tools: [search_tickets, list_intents, list_flows, list_boards]
---

# Zero-Touch Opportunity Mining

Mine recent tickets for the patterns most worth automating: candidates for intents and flows, ranked by volume × automatability, with the existing automation inventory checked so you don't propose what already exists.

## When to use

- "Where are our biggest zero-touch opportunities?"
- "Analyze previous tickets and suggest intents based on common trends."
- "What should we automate next to lift our zero-touch rate?"

## Steps

1. Confirm the period (default: last 60–90 days) and pull the resolved-ticket population with split searches per board; disclose caps.
2. Cluster into repeatable request/issue patterns (password resets, new-user requests, distribution-list changes, printer mapping, common alert classes, status inquiries).
3. Check the existing inventory with list_intents and list_flows — mark clusters already covered, partially covered, or uncovered.
4. Score each uncovered/partial cluster:
   - **Volume** — tickets per month in the cluster.
   - **Automatability** — how deterministic the resolution is: fixed steps, low judgment, verifiable inputs, no human approval legally required. Score high/medium/low with a one-line justification.
   - Rank by volume × automatability.
5. For the top 5, give: the pattern, monthly volume, estimated tech-time it consumes, the proposed mechanism (intent with variations, flow trigger, self-service KB deflection, or upstream fix that removes the tickets entirely), and one representative ticket.
6. Present as recommendations. Offer to draft an intent or flow for the #1 candidate only if the requester is an admin and asks — creation is a separate, confirmed step.

## Guardrails

- Never create or modify intents/flows as part of the mining run; this skill recommends. Creation requires explicit confirmation and admin tooling access (intent/flow tools are admin-only and may be hidden for this member — degrade to recommendations-only and say so).
- Sometimes the best "automation" is elimination — if a cluster traces to a fixable root cause (bad monitor threshold, missing license), recommend the fix over an intent.
- Exclude junk/NOC boards from the mining population unless alert handling is the stated target.
- Volume figures from capped searches are floors, not totals — disclose in a methodology note.
- Identity-sensitive patterns (password/MFA resets) must carry a note that any automation needs a verification step; never propose fully unattended credential handling.
