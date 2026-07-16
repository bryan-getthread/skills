---
name: Title Audit Trail
description: Wrap any ticket retitle so the PSA keeps a record — capture the existing title, apply the new one, and post a "Previous title:" note so nobody loses the original wording. Answers the commonly requested "don't let AI retitles erase what the client actually wrote" partner workflow.
category: Documentation
tools: [search_tickets, update_ticket, add_ticket_note]
---

# Title Audit Trail

Retitling a ticket makes the queue readable but destroys the client's original wording — which sometimes carried the only clue to what they meant. This skill wraps any retitle: it records the old title in a note before overwriting it, so the audit trail survives the improvement.

## When to use

- The desk uses Generate Title / normalization and wants the original subject preserved.
- "Whenever we rename a ticket, keep the old name somewhere."
- A retitle changed the meaning and a tech needs to see what the client first wrote.

## Steps

1. Read the ticket with `search_tickets` and capture the **current title exactly** before touching it.
2. Determine the new title (from the caller's instruction, or a normalization/board-convention skill such as `skills/triage-and-routing/board-scoped-title-rules` / `skills/triage-and-routing/alert-title-normalization`). If the new title would be identical to the current one, do nothing.
3. Apply the new title with `update_ticket`.
4. Post a plain-text note via `add_ticket_note`: `Previous title: <old title>` — verbatim, so the original wording is preserved in the PSA record.

## Guardrails

- Capture the old title **before** the `update_ticket` call — once overwritten it is gone. If you cannot read the current title first, do not retitle.
- The note preserves the old title verbatim, including any client phrasing — do not "clean up" the recorded original.
- Retitle + one audit note are the only writes. This skill never changes status, owner, priority, or classification.
- One audit note per retitle; if the title is unchanged, write nothing.
- Notes plain text — no markdown or emojis (PSA compatibility).

## Unattended (Flows) variant

- Fires on **ticket create** (typically paired with a Generate Title flow action) — a supported event. Because the skill itself reads the title before rewriting it, it captures the true original even in a flow that would otherwise overwrite it.
- Your entire reply is the audit note, verbatim: `Previous title: <old title>` — plain text, no narration. If the title is unchanged, output nothing.
- Deterministic stop: current title could not be read, or new title equals old → no write.
