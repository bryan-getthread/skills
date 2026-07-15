---
name: Linear Client Impact Queue
description: Rank open field-escalation issues in Linear by real client impact — how many tickets and clients each one is blocking — and propose re-prioritization to engineering with the evidence attached. Use for "which escalations hurt the most", "re-rank our Linear escalations by client impact", or "is engineering working on the right bugs".
category: Connectors
tools: [list_issues, get_issue, update_issue, list_comments, create_comment, list_issue_labels, search_tickets]
---

# Linear Client Impact Queue

Engineering prioritizes what it can see. This skill counts the tickets and clients actually waiting behind each field-escalation issue and turns that into a ranked queue — then puts the impact data on the issues where engineering will read it.

## When to use

- "Rank our open escalations by how many clients are affected."
- Weekly service-desk ↔ engineering sync prep.
- "Bug <issue id> feels bigger than its priority — prove it."

## Steps

1. `list_issues` filtered to the field-escalation label/project (confirm which label or project marks escalations on first use; remember it for the tenant).
2. For each open issue, measure impact from the ticket side: `search_tickets` for tickets referencing the issue id and for the issue's symptom signature (per-issue searches; capped results reported as "at least N"). Count distinct tickets, distinct clients, oldest waiting ticket, and any P1/VIP involvement.
3. Rank: distinct clients affected first, then ticket count, then age of oldest waiting ticket. Note trend where visible (new tickets this week vs prior).
4. Compare the ranking to current Linear priorities (`get_issue`). Flag mismatches both ways: high-impact/low-priority AND low-impact/high-priority (candidates to deprioritize honestly).
5. Present the queue: issue, current priority, proposed priority, tickets/clients waiting, evidence summary. For confirmed changes only: `create_comment` on the issue with the impact data (counts, clients as "<N> clients" not names, oldest-ticket age, example ticket refs), and `update_issue` priority if the member wants the change made rather than requested.
6. Offer the reverse artifact: a short summary for the service-desk side of which escalations engineering has picked up.

## Guardrails

- **Member-authenticated connector:** requires the member's Linear connection (scoped to their workspace access). Degrade by producing the ranked impact table for manual posting.
- Never change a Linear priority without explicit confirmation; default posture is comment-with-evidence and let engineering re-rank.
- Ticket counts from capped or signature-based searches are floors and labeled so — inflated impact numbers burn credibility fast.
- Client names generally stay out of Linear comments (engineering tools often have wider audiences); use counts, with names only if the member explicitly wants them.
- One impact comment per issue per run; update the previous comment's thread rather than stacking duplicates.
