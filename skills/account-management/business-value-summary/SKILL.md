---
name: Business Value Summary
description: Summarize the value delivered to a client this period in business terms — outcomes achieved, time saved, incidents prevented — as the backbone of a client-facing "what you got from us" story.
category: Account Management
tools: [search_tickets, search_clients]
---

# Business Value Summary

Turn a period of resolved tickets into the story a business owner actually cares about: what kept working, what got fixed before it hurt, and what it would have cost them without us. Produces a client-facing draft plus internal backup.

## When to use

- "What value did we deliver to <client> this quarter?"
- "Build the value slide for <client>'s renewal/QBR."
- "Client asked what they're paying us for — help me answer."

## Steps

1. Confirm the client with search_clients and the period. Default to the last full quarter.
2. Pull the period's resolved work with search_tickets. Exclude auto-resolved noise and merged threads.
3. Translate the work into **business outcomes**, not ticket language. Group resolutions into themes ("kept email flowing", "got new hires productive on day one", "recovered files that would have been lost") — one representative example per theme, told as a short plain-language story with no ticket IDs.
4. Estimate **time saved / disruption avoided** where the evidence supports it: number of people affected, rough downtime avoided, response times on urgent issues. Label every quantified figure as an estimate and state its basis in one clause ("~40 staff-hours, based on 20 password lockouts resolved within an hour").
5. Call out **incidents prevented or contained** — the proactive catches: a failing backup fixed before restore day, a security alert contained, a certificate renewed before expiry.
6. Note anything delivered **beyond the contract** — advice, small favors, out-of-scope saves — briefly and factually.
7. Output two clearly separated parts:
   - **Client-facing draft** — the value story: themes, examples, estimates. No ticket IDs, no internal names, no jargon.
   - **Internal backup** — for the AM only: which tickets support each claim, and where estimates are soft.

## Guardrails

- The client-facing section and the internal backup must never be mixed. No ticket IDs, technician commentary, or internal shorthand in anything the client sees.
- Every quantified claim needs a stated basis. Never invent dollar figures; time and disruption estimates only, clearly labeled as estimates.
- Do not claim prevention without evidence of the catch. "We prevented a breach" requires a thread showing the containment.
- One representative example per theme; resist listing everything.
- If searches hit a result cap, base the story on the sample and say so in the internal backup.
- If the period was genuinely thin, say so internally rather than inflating — a padded value story fails in the room.
