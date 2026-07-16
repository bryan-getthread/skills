---
name: Zapier Monday Boards
description: For clients who run their operations on Monday.com — mirror project-worthy tickets as items on the client's agreed board, sync status through their column values, and close the loop on completion. Use for "add this to their Monday board", "what's the status on their Monday item", or clients coordinating rollouts in Monday.
category: Connectors
tools: [search_tickets, add_ticket_note, update_ticket, "zapier: monday.com"]
---

# Zapier Monday Boards

Monday.com shops see the world as boards, groups, and column values — a ticket that isn't an item on their board doesn't exist to them. This skill mirrors project-worthy tickets as items on the client's agreed board, writes status into *their* columns using *their* labels, and keeps both records pointing at each other.

## When to use

- "<client> tracks the rollout on a Monday board — mirror ticket <number> there."
- A ticket is one work item in a client-led initiative managed in Monday.com.
- "Pull the current state of their Monday item onto our ticket."
- The client asks the MSP to keep their board current as tickets progress.

## Steps

1. **Verify before promising (Monday.com's Zapier inventory is not pre-verified in this library):** run the Zapier Action Discovery pattern first — confirm actions equivalent to "Create Item", "Create Update" (item comment), "Change Column Value", and an item/board Find lookup exist and are enabled before describing the workflow. Missing pieces change the design — e.g. no column-write action means status lives in updates (comments) only; say which shape is actually available.
2. Confirm the target: board, group, and which columns the client wants written (status column and its exact label set, date column, a text column for the ticket reference). Column labels are the client's taxonomy — use their exact values; a guessed status label either errors or silently miscategorizes their work.
3. **Qualify the ticket:** mirror project-worthy work tied to the client-led initiative, not routine support volume. Unsure → ask the member.
4. **Find before create:** search the board for an existing item carrying this ticket's reference. Found → update/comment on that item; duplicate items break the client's board metrics.
5. Create the item: client-safe name in their convention, ticket reference in the agreed column, initial status per their label set. Show the composed item for confirmation, then create. Capture the returned item ID and record it on the ticket via `add_ticket_note` (plain text) — the permanent handle.
6. Status sync outbound: on material ticket progress, set the client's status column to the matching label (only labels from their set) and/or post an update on the item — client-safe, dated, ticket-referenced.
7. Status sync inbound: on request or per pass, read the item and relay material changes (status column moved, due date changed, updates from their team) onto the ticket via `add_ticket_note` with "per Monday item <ID>" provenance.
8. **Completion loop:** ticket resolves → post the outcome as an update and set the status column to the client's done-equivalent label only if the agreement says the MSP does so; otherwise post "done from our side". Client marks the item done first → note it on the ticket and confirm with the requester before resolving.

## Guardrails

- **Member-connected Zapier required** (the client's Monday.com workspace, standing client-granted access). Degrade: output the composed item name, column values, and update text for someone with board access, and note on the ticket that the mirror is pending.
- Their board, their columns, their labels: write only the agreed columns with values from the client's exact label set; never add columns, rearrange groups, or touch items this skill doesn't own.
- Everything written is client-visible: professional wording, no internal commentary, no credentials, no other clients' data.
- Item-ID-on-ticket is non-negotiable — future syncs address the ID, not a name search.
- When in doubt whether a ticket belongs on their board, ask; over-mirroring is how clients revoke board access.
- Task cost: each Zapier call = 2 Zapier tasks — find + create per mirror, one call per column write or update; batch status changes into one touch where possible and never poll.
