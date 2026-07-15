---
name: Ticket QA Review
description: Review a completed ticket against your closure standard, confirm the issue was genuinely resolved, and pass or bounce it.
category: QA & Closure
tools: [search_tickets, add_ticket_note, update_ticket, list_ticket_statuses, run_assistive_ai]
---

# Ticket QA Review

**When to use:** A ticket has reached completed or ready-to-close status and needs a quality check before it closes.

**What you get:** A pass/fail QA result posted as an internal note. Passes proceed to close; fails go back with a reason.

## Steps

1. Retrieve full ticket detail: notes, time entries, categorization, priority, and resolution.
2. Check for genuine resolution evidence: an explicit customer confirmation, or a technician note documenting verbal confirmation. Vague wording like 'should be fine now' does not count.
3. Score documentation quality: correct board, status, type/subtype, complete intake notes, accurate time entries.
4. If it passes, allow closure and, where configured, trigger the CSAT survey.
5. If it fails, move it back to the prior status and add an internal note explaining exactly what is missing.

## Guardrails

- Score each criterion from the defined source of truth only. If evidence is not there, score it zero. Do not infer.
- Post only the result and a one-line note per criterion. Keep internal reasoning internal.

## Consolidates

Quality assurance scoring, resolution compliance audit, completed-ticket confirmation checks, and quick QA review skills.
