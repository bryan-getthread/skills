---
name: Feature Request Rollup
description: Classify "wish" tickets as feature requests, dedupe them against existing Linear issues, and either +1 with client context or create a new issue — building a demand-weighted product signal.
category: Escalation
tools: [search_tickets, add_ticket_note, list_issues, get_issue, create_issue, create_comment, list_issue_labels, create_issue_label, list_teams]
---

# Feature Request Rollup

Turns scattered "it would be great if…" tickets into a demand-weighted product backlog: every request lands on exactly one Linear issue, each carrying the roster of clients asking for it — so product decisions ride on counted demand, not the loudest recent voice.

## When to use

- "This ticket is a feature request — log it with product/engineering."
- Periodic sweep: "collect this month's feature requests and roll them up into Linear."
- Product asks "how many clients have asked for <capability>?"

## Steps

1. Confirm the Linear connector is available (member-authenticated; if absent, output the rollup as text and stop before any Linear writes).
2. Collect candidates: the ticket in front of you, or a search_tickets sweep for wish-shaped language ("feature request", "would be nice", "can it do", "any way to") over the review window. Split searches per phrase; disclose result caps.
3. Classify honestly. Keep only genuine product capability requests. Route out: bugs (that's the Engineering Escalation to Linear skill), config/training gaps the desk can just fix, and one-off custom work (project/sales conversation). List the routed-out items with their destination.
4. Normalize each request to one line of product language: what the user is trying to accomplish, not their proposed implementation.
5. Dedupe against Linear: list_issues (feature-request label / product team) and compare by capability, not phrasing. Be conservative — two requests are the same only when the underlying capability is; near-matches are flagged for a human call, not silently merged.
6. **Existing issue** → create_comment (+1) carrying: <client> name, the ticket reference, the user's goal in their own words (sanitized), and any deal/renewal stakes the ticket documents. Update the running demand count in the comment ("now 6 clients").
7. **New request** → create_issue with the feature-request label (create_issue_label once if the label doesn't exist; confirm team via list_teams): capability description, first requesting client + ticket reference, and the demand count seeded at 1. Confirm before creating.
8. Cross-reference: plain-text note on each rolled-up ticket with its Linear issue ID, so the requester can be notified if it ships (the Linear Fix-Shipped Loop skill picks this up).
9. Output the rollup table: request → Linear issue → clients-asking count, new vs +1'd, plus the routed-out list.

## Guardrails

- One capability, one issue. Wrong merges destroy the demand signal — when unsure whether two requests match, ask rather than merge.
- Demand counts only from documented, distinct clients; never pad with "probably others."
- Client context in Linear comments is sanitized: client name and goal, no end-user personal data, no verbatim internal grumbling.
- Bugs are not feature requests — misfiling inflates the backlog and buries defects.
- Confirm before creating issues or labels; ticket notes are plain text; requires member-connected Linear for writes.
