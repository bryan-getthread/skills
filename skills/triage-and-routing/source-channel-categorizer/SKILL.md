---
name: Source Channel Categorizer
description: Set a ticket's category/type from the channel it came in on (voice→Phone, chat→Chat, email→Email) on create, so downstream PSA automations that key off category fire correctly. Answers the commonly requested "tag tickets by how they arrived" partner workflow.
category: Triage & Routing
tools: [search_tickets, update_ticket, list_boards, add_ticket_note]
---

# Source Channel Categorizer

Downstream PSA automations and reports often key off a ticket's category/type, but tickets arrive uncategorized regardless of channel. This skill reads the intake source and stamps the matching category on create — voice becomes Phone, chat becomes Chat, email becomes Email — so channel-based routing and reporting work.

## When to use

- PSA automations or SLAs branch on category but new tickets come in blank.
- "Tag phone tickets as Phone and chat tickets as Chat automatically."
- Making channel visible in reporting without a tech setting it by hand.

## Steps

1. Read the ticket with `search_tickets` and determine its intake source/channel (voice, chat, messenger, email, portal).
2. Map the source to this board's **documented category/type value** for that channel (configured alongside the skill; `list_boards` to confirm the board). Use only category/type values that exist on the board — see `skills/psa-specific/cw-type-subtype-item` and `skills/psa-specific/autotask-ticket-categories`.
3. If the source is unambiguous and maps to a board-valid category, set it with `update_ticket`. If the category is already correct, do nothing.
4. Post a plain-text note via `add_ticket_note`: the detected source and the category applied.

## Guardrails

- **Only board-valid values.** Never invent a category/type; PSAs reject unknown values on sync. If the channel has no mapped board value, leave it and note the gap.
- Set category only when the source is unambiguous — if the channel can't be determined, do nothing rather than guessing.
- Do not overwrite a category a human already set to something more specific just to match the channel; if the existing category conflicts, note it and leave it for review.
- Category + one note are the only writes. No status/priority/owner changes.
- Notes plain text (PSA compatibility).

## Unattended (Flows) variant

- Fires on **ticket create**, using the flow's **source** condition — a supported filter attribute — so a per-source flow can route straight to the right category.
- Your entire reply is the note, verbatim: `CHANNEL CATEGORY SET. Source: <source>. Category: <value>.` or `NO ACTION. Reason: <source unknown | no board value | already set>.`
- Deterministic stops: source ambiguous → no action; no board-valid mapping → no action; category already correct → no write.
