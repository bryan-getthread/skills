---
name: CW Type / Subtype / Item Classification
description: For desks synced to ConnectWise Manage — classify tickets with CW's three-level Type → Subtype → Item taxonomy using only values that exist in the board's configuration, never invented ones.
category: PSA-Specific
tools: [search_tickets, list_boards, update_ticket, add_ticket_note]
---

# CW Type / Subtype / Item Classification

Applies to: **ConnectWise Manage**. CW classifies tickets on a three-level, board-scoped taxonomy: Type → Subtype → Item. Only combinations configured on the board are valid; an invalid combo either fails the sync or silently drops. This skill applies the taxonomy with decision discipline.

## When to use

- Classifying a new ticket on a CW-synced board ("set the type/subtype", "categorize this").
- A closure QA gate requires Type/Subtype/Item before the ticket can close.
- Auditing recently closed tickets for missing or defaulted classification.

## Steps

1. Re-fetch the ticket with `search_tickets` and read the full thread — classify from the body of the request, never the title alone (see `skills/triage-and-routing/ticket-triage`).
2. Establish the valid value set. Thread does not expose CW's setup tables directly, so derive it from evidence, in order of preference: the desk's documented taxonomy sheet (see `skills/psa-specific/psa-field-mapping-doc`); values observed on recently closed tickets on the **same board** via `search_tickets`; or ask the reviewer. Never guess a value you have not seen.
3. Pick Type first, then Subtype, then Item — each level narrows the previous one. Do not pick an Item that you have not seen paired with that Subtype: CW combos are configured pairwise, and unpaired combos are invalid even when each value exists individually.
4. If the ticket genuinely spans two types, classify by the primary actionable issue and note the secondary one; consider `skills/triage-and-routing/multi-issue-ticket-splitter` instead of a mushy classification.
5. If no observed combination fits, say so explicitly and leave classification to a human. A wrong classification poisons CW reporting and agreement routing; an empty one is at least visible.
6. Propose Type/Subtype/Item with a one-line justification; apply with `update_ticket` only after confirmation.

## Guardrails

- **Never invent values.** Only use Type/Subtype/Item values evidenced from the board's own tickets or the desk's documented list. CW rejects or mangles unknown values on sync.
- **Sync lag:** re-fetch full ticket detail before writing — the ticket may have been classified, moved, or closed in CW since you last saw it.
- Taxonomy is board-scoped: values valid on one CW board are not valid on another. Re-derive when the ticket moves boards.
- Do not default to a catch-all value ("General / Other / Other") to get past a gate — flag the gap instead.
- If classification drives agreement or billing routing on this desk, say so in the proposal and get explicit confirmation.

## Cross-references

- Generic classification tree: `skills/triage-and-routing/intake-classification-tree`
- Building the desk's taxonomy cheat sheet: `skills/psa-specific/psa-field-mapping-doc`
