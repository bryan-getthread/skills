---
name: Expansion Opportunity Scan
description: Mine a client's ticket history for expansion signals — recurring issues that justify a project or upsell, and training gaps that justify a service offering.
category: Account Management
tools: [search_tickets, search_clients]
---

# Expansion Opportunity Scan

Find the revenue hiding in the ticket queue: patterns that a project would eliminate, and user struggles that a training or managed-service offering would address. Output is a qualified internal opportunity list, not a pitch.

## When to use

- "Any upsell opportunities at <client>?"
- "What projects should we be proposing to <client> based on their tickets?"
- "Scan my accounts for expansion candidates."

## Steps

1. Confirm scope (one client or the portfolio) and window with search_clients. Default to the last 6 months.
2. With search_tickets, hunt two distinct signal families — run separate searches per family, per client:
   - **Recurring issues → project/upsell:** the same failure mode repeating (aging hardware, capacity limits, a flaky line-of-business app, an unmanaged system generating incidents). Each recurrence cluster is a candidate for a project, hardware refresh, license upgrade, or scope expansion.
   - **Training gaps → service opportunity:** clusters of how-do-I tickets, repeated user error on the same tool, or one team generating disproportionate basic requests. Candidates for training engagements or an expanded support tier.
3. For each opportunity, produce a qualified entry:
   - The pattern, with exactly one representative example in plain language (no ticket IDs).
   - Rough frequency and effort cost to the desk ("roughly monthly, ~2 hours each" — mark as estimate).
   - The proposed offering, one line.
   - Why the client would care, phrased in their business terms.
4. Rank opportunities by evidence strength times likely deal size, strongest first.
5. End with a note on anything that looks like an opportunity but is really a service-quality problem we should fix for free — do not disguise remediation as upsell.

## Guardrails

- Internal only. This list informs a proposal; it is not itself client-facing.
- Every opportunity must trace to ticket evidence. No generic MSP upsell boilerplate ("consider security awareness training") without threads to back it.
- Never flag on volume alone — a busy client is not automatically an expansion target; the pattern, not the count, makes the case.
- Be honest about the free-fix line: if the recurring issue stems from something we misconfigured or under-delivered, say so instead of proposing to charge for the cure.
- Frequency and effort figures are estimates from ticket data — label them, and note any result caps hit.
