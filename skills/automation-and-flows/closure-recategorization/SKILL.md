---
name: Closure Recategorization
description: When a ticket enters a resolution status, re-read the whole thread and correct its type/subtype/item and category to match what the work actually was — using only values valid on that board. Answers the commonly requested "fix the classification at close so reporting is right" partner workflow.
category: Automation & Flows
tools: [search_tickets, list_boards, update_ticket, add_ticket_note]
---

# Closure Recategorization

Tickets are classified at intake from a one-line request and rarely revisited. By the time a ticket resolves, the thread reveals what it really was. This skill re-reads the resolved thread and corrects type/subtype/item and category so closed-ticket reporting reflects reality — using only classification values that exist on the board.

## When to use

- A ticket just entered a resolution/closed-family status and its intake classification looks wrong.
- "Fix the categorization on resolved tickets before they sync out for reporting."
- A flow on close should re-classify from the full thread rather than the opening line.

## Steps

1. Re-fetch the ticket with `search_tickets` and read the **entire** thread — the resolution, the work notes, and the final client-visible reply. Classify from what was actually done, not the opening summary.
2. Establish the valid value set for this board. Thread does not expose PSA setup tables directly, so derive it from evidence in order of preference: the desk's documented taxonomy sheet, values observed on recently closed tickets on the **same board** via `search_tickets`, or ask the reviewer. Never use a value you have not seen on this board (`list_boards` to confirm the board context). See `skills/psa-specific/cw-type-subtype-item` for ConnectWise's pairwise Type→Subtype→Item rules and `skills/psa-specific/autotask-ticket-categories` / `skills/psa-specific/halo-sla-and-priorities` for the other PSAs.
3. Compare the current classification to the evidence-based one. If they already agree, do nothing.
4. If they differ, correct type/subtype/item and category with `update_ticket`, using only board-valid values (respect pairwise validity — an Item must have been seen with its Subtype).
5. Post a plain-text audit note via `add_ticket_note`: old classification, new classification, and the one-line evidence ("resolution shows this was a mailbox permissions fix, not a hardware issue").

## Guardrails

- **Never invent values.** Only apply type/subtype/item/category values evidenced from the board's own tickets or the desk's documented list — PSAs reject or silently drop unknown values on sync.
- Classification is board-scoped: values valid on one board are not valid on another. Re-derive if the ticket also moved boards.
- Do not default to a catch-all ("General / Other") to look tidy — if no observed value fits, leave it and flag the gap in the note.
- Classification and the audit note are the only writes. Never reopen, re-close, or change status/owner/priority here.
- If classification drives agreement or billing routing on this desk, say so in the note and get confirmation in attended mode.
- Notes are plain text — no markdown or emojis (PSA compatibility).

## Unattended (Flows) variant

- Fires on the **event of entering a resolution status** (a supported status-change condition) — never on a timer or "N hours after close".
- Your entire reply is the audit note, posted verbatim: `RECLASSIFIED. Was: <old>. Now: <new>. Evidence: <one line>.` or `CLASSIFICATION UNCHANGED. <one line>.` — no narration, plain text.
- Deterministic stops: no board-valid value fits → leave unchanged and note the gap; current classification already matches evidence → unchanged; thread too thin to reclassify with confidence → unchanged.
- One reclassification per closure event. Never invent a value to satisfy a downstream gate.
