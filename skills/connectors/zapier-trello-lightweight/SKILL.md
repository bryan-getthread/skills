---
name: Zapier Trello Lightweight
description: Lightweight ticket mirror for small clients who live on a Trello board — one card per project-worthy ticket in the agreed list, comments for status, card moved or checked off on completion. Use for "put a card on their Trello", "small client wants to see progress on their board", or the simplest possible client-visible mirror.
category: Connectors
tools: [search_tickets, add_ticket_note, "zapier: Trello"]
---

# Zapier Trello Lightweight

The small-client version of a ticket mirror: no columns to map, no taxonomy to negotiate — a card in the agreed list, a comment when something material happens, and the card moved to Done when the work finishes. Deliberately minimal, because for a five-person client a Trello board they already look at beats any portal they don't.

## When to use

- "<client> just wants to see our work on their Trello board — add a card for this ticket."
- Small clients without a PSA-facing portal habit who already live in Trello.
- "Update the Trello card for ticket <number>" / "they moved the card — check it."

## Steps

1. **Verify before promising (Trello's Zapier inventory is not pre-verified in this library):** run the Zapier Action Discovery pattern first — confirm actions equivalent to "Create Card", "Add Comment to Card", "Move Card to List" (or "Update Card"), and a "Find Card" lookup exist and are enabled. If move/update is missing, the completion loop degrades to a final comment — say so up front.
2. Confirm the target: the client's agreed board and the intake list ("MSP" / "In Progress" — whatever the client named it) plus their done-list name. This comes from the mirroring agreement or from asking, never from guessing among their boards.
3. **Qualify:** mirror project-worthy or client-visible work items, not routine volume. Trello boards are small; ten cards of noise kills the whole point.
4. **Find before create:** `zapier: Trello "Find Card"` on the board for this ticket's reference. Found → comment on the existing card instead of creating a twin.
5. Create the card: short client-safe title, description with what's being done and the Thread ticket reference. Show for confirmation, then create. Capture the card ID/URL and record it on the ticket via `add_ticket_note` (plain text).
6. Status: on material progress (started, blocked, waiting on client, resolved) add a card comment — one or two client-safe sentences, dated. Keep it lightweight; this is a glanceable board, not a work log.
7. Inbound: on request or per pass, read the card — if the client moved it, commented, or archived it, relay that onto the ticket via `add_ticket_note` with "per Trello card" provenance.
8. **Completion:** ticket resolves → comment the outcome and move the card to the client's done list (or check off/archive per their convention). Client moves it to done first → note it on the ticket and confirm with the requester before resolving on the Thread side.

## Guardrails

- **Member-connected Zapier required** (the client's Trello board via standing, client-granted access). Degrade: output the card title, description, and comment text for someone with board access to paste, and note on the ticket that the mirror is pending.
- Their board: touch only cards this skill created or was pointed at, in the agreed lists. Never reorganize their lists, labels, or other cards.
- Card content is fully client-visible (and Trello boards are sometimes public-link shared): client-safe wording only, no internal commentary, no credentials, no other clients' names.
- Card-ID-on-ticket is required; a lightweight mirror still needs a durable handle.
- Lightweight means lightweight: one card per ticket, comments on material change only, no checklists-of-internal-steps unless the client asked.
- Task cost: each Zapier call = 2 Zapier tasks — find + create per mirror, one call per comment or move. For a small client this should be single-digit tasks per ticket lifecycle; if it isn't, the mirror is too chatty.
