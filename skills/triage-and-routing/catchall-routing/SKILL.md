---
name: Catchall Routing
description: Identify the correct client and contact for a ticket that landed in a catchall or no-company mailbox — including forwarded mail and vendor alerts — then reassign it or hand back candidates.
category: Triage & Routing
tools: [search_tickets, search_clients, search_contacts, assign_contact, update_ticket, add_ticket_note, create_ticket]
---

# Catchall Routing

Route a ticket that arrived with no company (or on a catchall contact) to the real client and contact, using an explicit evidence ladder — or leave it alone when the evidence is not there.

## When to use

- A ticket has no company assigned or sits on a catchall/no-company contact.
- Mail was forwarded in ("FW:") by a tech or client and the ticket is attributed to the forwarder, not the original sender.
- A vendor or monitoring alert landed in the catchall and needs its client extracted from the alert body.
- Periodic sweep of the catchall/intake board to remap misattributed tickets.

## Steps

1. Read the ticket with `search_tickets`: title, description, earliest message, and full headers/quoted text if present.
2. Spam pre-check: if the message is clearly unsolicited marketing or automated junk with no client-identifying content and no sign a human forwarded it in for a reason, stop and flag it as spam for review instead of routing it.
3. Extract identity clues in this order of strength (the evidence ladder):
   a. Email domain of the true sender (strongest),
   b. An explicit company name stated in the body or alert fields,
   c. A person's name alone (weakest — never sufficient by itself).
4. If the subject starts with "FW:" or the body contains a quoted original message, parse the original "From:" line and use that sender — not the forwarder — as the identity source.
5. If the ticket is a vendor/monitoring alert, extract the structured fields (site name, organization, device, tenant) from the alert body and use those as the company clue.
6. Resolve the company with `search_clients` on the domain or extracted name; then find the contact with `search_contacts` scoped to that company. If the sender has no contact record, propose the closest match or note that one needs creating — do not attach to a lookalike.
7. If the current wrong assignment must be cleared before the correct one can apply on this tenant, unassign first (set the ticket back to no-company/catchall) and then apply the correct company and contact.
8. Apply the match with `update_ticket` and `assign_contact` only when exactly one company candidate fits at domain or explicit-name strength. Otherwise present the candidates and ask.
9. If the tenant's PSA sync rejects company changes on an existing ticket, use the close-and-recreate path: `create_ticket` under the correct company with the original text and a plain-text note cross-referencing both ticket numbers, then close the original — only with confirmation in attended use.
10. Post a plain-text internal note with `add_ticket_note` stating what was matched, which evidence rung was used, and what changed.

## Guardrails

- Never assign on name similarity alone. A matching first/last name without a domain or explicit-company confirmation is low confidence → make no change.
- Ambiguity (two or more plausible companies) → no change; list candidates instead.
- A ticket synced to a PSA must always have a company; never leave a PSA-bound ticket companyless — if no match is possible, route it to the designated internal/catchall client per desk policy and flag it.
- Close-and-recreate is destructive-adjacent: in attended use, confirm before doing it; preserve the original text and cross-reference both numbers in plain text.
- Notes destined for the PSA are plain text — no markdown, no emojis.
- Do not invent companies, contacts, or ticket numbers. If searches may have hit result caps, say so.

## Unattended (Flows) variant

- Act only at domain-match or explicit-company strength; on anything weaker, make no change and post one plain-text internal note: `CATCHALL ROUTING: no confident match. Evidence found: <clues>. Left unassigned for human review.`
- Never use the close-and-recreate path unattended.
- Spam branch: do not close unattended; flag via note only.
- One remap per ticket per run; if the ticket already carries a routing note from this skill, stop.
- Your entire reply is the internal note — no narration, no questions.

## Consolidates

CatchAll-to-contact mapping, catchall repair, unassign-first remaps, spam pre-check intake, FW:-header original-sender parsing, vendor-alert field extraction, company/contact identification, no-company enrichment, close-and-recreate remaps, and reroute-to-correct-client skills (28 mined variants).
