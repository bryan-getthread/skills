---
name: CW Time Entry Conventions
description: For desks synced to ConnectWise Manage — log time entries that survive the CW sync: correct work role/work type, deliberate billable flag, agreement application, and plain-text notes.
category: PSA-Specific
tools: [search_tickets, log_time_entry, add_ticket_note]
---

# CW Time Entry Conventions

Applies to: **ConnectWise Manage**. In CW, a time entry is a billing event: work role and work type set the rate, the billable flag and agreement application decide whether the client is invoiced. This skill logs time that lands correctly on the CW side.

## When to use

- Logging time on a CW-synced ticket ("log 30 minutes on this", call wrap-ups).
- A tech asks which work role/work type or billable flag to use.
- Cleaning up time entries that failed to apply to an agreement or billed unexpectedly.

## Steps

1. Re-fetch the ticket with `search_tickets` before logging — confirm it is still open and on the expected board. Time logged against a closed or moved ticket may not apply to the agreement.
2. Determine the client's coverage before setting the billable flag: what agreement covers this work, and is this ticket's work inside it? Use `skills/psa-specific/cw-agreement-aware-triage` if coverage was not established at intake. If coverage is unknown, ask — do not default silently to billable or non-billable.
3. Choose work role and work type from the desk's documented conventions (or values observed on comparable tickets). These set the invoice rate in CW; never invent them.
4. Write the time-entry note in **plain text only** — no markdown, no bullets-as-asterisks that render as noise, no emojis. Structure: what was done, outcome, next step. Follow `skills/documentation/note-format-standard` and the zero-assumption rule: never record a recommendation as a completed action.
5. Estimate duration honestly from evidence (call length, timestamps). Do not round up "to be safe" and do not fabricate start/end times.
6. Log with `log_time_entry`. State in your output exactly what was logged: duration, billable flag, and the note text. If the desk needs an internal-only remark, put it in a separate internal note via `add_ticket_note`, not inside the billable time note.

## Guardrails

- **The time note may appear on the client's invoice.** Write every time-entry note as if the client's finance team will read it. No internal commentary, no speculation, no other clients named.
- **Sync lag:** re-fetch full ticket detail before logging; the ticket's status, board, or agreement linkage may have changed in CW.
- Billable flag is a deliberate decision, never a default. When coverage is ambiguous, log the time with the desk's documented fallback and flag it via `skills/finance-and-billing/out-of-scope-billing-flag`.
- Plain text only in anything that syncs to CW.
- Do not delete or rewrite an existing time entry to fix a billing problem — propose the correction and route it to a human; invoiced time may already be locked in CW.

## Cross-references

- Note quality: `skills/documentation/note-format-standard`, `skills/documentation/time-entry-cleanup`
- Coverage decisions: `skills/psa-specific/cw-agreement-aware-triage`
