---
name: Time Entry Cleanup
description: Turn raw, rough time notes into clean, standardized time entries and a work summary.
category: Documentation
tools: [search_tickets, log_time_entry, add_ticket_note]
---

# Time Entry Cleanup

**When to use:** A technician pastes messy time notes, or wants a formatted time/work summary for a ticket.

**What you get:** Standardized time entries in your format, plus a short work summary suitable for the ticket.

## Steps

1. Read the raw notes and pull context from the ticket thread automatically.
2. Normalize each entry: what was done, in clear past-tense, at a consistent level of detail.
3. Apply your time-entry template and rounding rules.
4. Output the cleaned entries and a brief overall work summary.

## Guardrails

- Preserve the technician's meaning. Clean up wording, do not fabricate work.
- Keep billable detail accurate to what the notes describe.

## Consolidates

Time entry summary and raw-time-note reformatting skills.
