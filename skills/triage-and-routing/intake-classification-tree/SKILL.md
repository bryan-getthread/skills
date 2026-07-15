---
name: Intake Classification Tree
description: Walk a new ticket through the Incident / Request / Problem decision tree and recommend the matching type, subtype, and item so intake classification stays consistent.
category: Triage & Routing
tools: [search_tickets, list_boards, update_ticket]
---

# Intake Classification Tree

Consistent classification at intake makes every downstream report trustworthy. This skill walks the desk's decision tree — Incident, Request, or Problem first, then type/subtype/item — and recommends one classification with the reasoning shown.

## When to use

- A new ticket needs its type/subtype/item set and the tech wants a consistent call.
- "Is this an incident or a request?"
- Intake review found inconsistently classified tickets to re-run through the tree.

## Steps

1. Read the ticket body and thread with `search_tickets` — classify from what is described, not the title.
2. Top of the tree — pick exactly one:
   - **Incident**: something that worked is now broken or degraded ("can't", "down", "error", "stopped working").
   - **Request**: the user wants something new or changed that is not broken (access, equipment, how-do-I, moves/adds/changes).
   - **Problem**: the underlying cause behind recurring incidents, or a proactively identified defect — typically opened by the desk, not the client.
3. Descend to type/subtype/item using the desk's category tree (as configured for this tenant): affected system first (email, endpoint, network, identity, application, printing, telephony), then the specific service or action.
4. Edge rules: a request blocked by an error is an Incident on the blocking error; "it broke because we changed it" is an Incident referencing the change; multiple recurrences of the same incident → recommend also opening/linking a Problem, do not reclassify the incident itself.
5. Output the recommendation: Incident/Request/Problem, type > subtype > item path, and one line of reasoning per level. Apply with `update_ticket` only if the tech confirms.

## Guardrails

- Recommend first; write only after confirmation. Classification drives SLAs and billing — a silent wrong write is expensive.
- Choose only from the tenant's actual category values; if the tree does not contain a fitting item, recommend the closest parent and flag the gap rather than inventing a value.
- One classification per ticket: if the ticket genuinely contains both an Incident and a Request, that is a splitter case — say so instead of blending.
- Do not let the requester's urgency influence type — priority is a separate decision from classification.
