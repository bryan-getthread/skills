---
name: SOW Drafting
description: When project-shaped work needs a brief scope-of-work drafted from ticket or project context — deliverables, assumptions, exclusions, and a T&M vs fixed recommendation.
category: Sales & Quoting
tools: [search_tickets, search_clients, search_knowledge_base, add_ticket_note]
---

# SOW Drafting

Draft a brief, honest scope-of-work from what the tickets actually say: concrete deliverables, the assumptions the price depends on, explicit exclusions, and a reasoned T&M-vs-fixed recommendation — ready for the sales owner to price and polish.

## When to use

- "Draft a SOW for the office move work on this ticket."
- "Turn this project conversation into a scope of work."
- "We're quoting the migration — write up scope, assumptions, exclusions."

## Steps

1. Gather the source context: read the ticket(s) in full via `search_tickets`, and check `search_knowledge_base` for any standard project template or prior SOW the desk keeps. Resolve the client with `search_clients`.
2. Extract what is actually known: the outcome the client asked for (quote their words where useful), environment facts stated in the thread, quantities, sites, deadline drivers.
3. Draft the SOW in five sections, plain language:
   - **Objective** — one paragraph, the client's outcome.
   - **Deliverables** — numbered, each verifiable ("X configured and tested", not "assist with X").
   - **Assumptions** — everything the effort estimate depends on that hasn't been verified (access provided, counts accurate as stated, work during business hours). Every unverified fact from step 2 becomes an assumption, not a silent premise.
   - **Exclusions** — the adjacent work this SOW does NOT include (the usual scope-creep vectors for this project type: end-user training, legacy cleanup, third-party licensing, out-of-hours cutover unless listed).
   - **Engagement model recommendation** — T&M when discovery is incomplete or the environment is unknown; fixed when deliverables and counts are verified and the desk has done this project type repeatedly. Give the reasoning in 2–3 lines.
4. Leave pricing as placeholders (<rate>, <estimate range>) unless the requester supplies figures — effort and price belong to the sales owner.
5. Output the draft in chat and offer to attach it as a plain-text internal note via `add_ticket_note`. Hand off: note what the sales owner must verify before sending (the assumptions list doubles as their checklist).

## Guardrails

- This is a draft for internal review, never a client-ready contract — no sending, and no legal terms (liability, warranties, payment terms) invented by the skill.
- Never state a contractual conclusion (what the existing agreement covers or excludes) as fact without citing the agreement evidence; scope boundaries against the current agreement are flagged for the sales owner.
- Deliverables come from evidence in the tickets or the requester — do not pad scope with plausible-sounding items the client never raised, and do not promise outcomes the thread shows are uncertain.
- Anything unverified is an assumption in writing. An assumption silently treated as fact is how fixed-price projects lose money.
- Sanitize: use role placeholders for client staff; no credentials or internal rates in the SOW body.
