---
name: Catchall Routing
description: Identify the correct client and contact for a ticket that landed in a catchall or no-company mailbox, then reassign it.
category: Triage & Routing
tools: [search_tickets, search_clients, search_contacts, assign_contact, update_ticket]
---

# Catchall Routing

**When to use:** A ticket has no company assigned, or landed on a catchall contact, and needs to be routed to the real client.

**What you get:** The ticket reassigned to the correct company and contact, or a short list of candidates when the match is ambiguous.

## Steps

1. Read the ticket context with `search_tickets` (title, description, earliest message).
2. Extract identifying clues: sender name, email address, company name, or domain.
3. Resolve the company with `search_clients` on the name or email domain.
4. Find the matching contact with `search_contacts` scoped to that company.
5. Present the match for confirmation, then apply it with `update_ticket` and `assign_contact`.

## Guardrails

- When more than one company or contact matches, show the options and ask rather than guessing.
- If nothing identifies the client, leave it unassigned and flag it. Do not force a match.

## Consolidates

CatchAll-to-contact mapping, catchall repair, company/contact identification, no-company enrichment, and reroute-to-correct-client skills.
