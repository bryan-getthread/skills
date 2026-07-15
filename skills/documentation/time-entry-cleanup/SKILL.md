---
name: Time Entry Cleanup
description: Turn raw, rough time notes into clean, standardized time entries — client-facing and internal versions — recording only work the engineer explicitly stated was done.
category: Documentation
tools: [search_tickets, log_time_entry, add_ticket_note]
---

# Time Entry Cleanup

Normalize messy time notes into the desk's standard time-entry format, splitting
client-facing wording from internal detail, without ever inventing or upgrading work.

## When to use

- A technician pastes rough shorthand ("rst pw, tested ok, told usr") and wants a proper time entry.
- "Clean up my time notes for this ticket."
- End-of-day: several raw entries need standardizing before invoicing review.
- A call wrap-up needs to become a billable entry with a duration.

## Steps

1. Read the raw notes; pull surrounding context from the ticket thread with `search_tickets` only to resolve references (which ticket, which <user>, which <device>) — never to add work items.
2. Rewrite each entry in clear past tense using the fixed field order:
   1. **Issue** — what was reported.
   2. **Work performed** — what the engineer explicitly stated they did.
   3. **Result** — outcome as stated.
   4. **Next steps** — only if the engineer stated one.
3. Apply the **zero-assumption rule**: include only what the engineer explicitly said was done. Never convert a recommendation, intention, or "should" into a completed action ("recommended a reboot" must NOT become "rebooted"). If notes are ambiguous, ask or mark it `[unclear — confirm]`.
4. Apply banned-word replacements per house style — typical set: "fixed" → "resolved"; "issue with the guy" → "issue reported by <user>"; "messed up" → "misconfigured"; "should be fine" → "verified working" **only if verification was stated**, otherwise "pending user confirmation". Honor the desk's own banned list when provided.
5. Produce two versions when requested:
   - **Client-facing** — plain professional language, no internal shorthand, no blame, no vendor grumbling, plain text (PSA-safe).
   - **Internal** — full technical detail, hypotheses, and follow-up flags.
6. Output the cleaned entries with durations. Only write with `log_time_entry` / `add_ticket_note` after the technician confirms the text and the time amount.

## Guardrails

- **Zero assumption is absolute.** No inferred work, no inferred verification, no inferred durations. If a duration was not stated, ask — do not estimate silently.
- Preserve the technician's meaning; clean wording only. Billable detail must stay accurate to what the notes describe.
- Never put internal-only remarks (blame, client friction, security suspicions) into the client-facing version.
- Confirm before logging: entries affect billing. When in doubt, output text only and let the tech submit.

## Consolidates

Time-entry summary, raw-time-note reformatting, call wrap-up, and banned-word note-style skills.
