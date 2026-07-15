---
name: Ticket QA Review
description: Grade a completed or ready-to-close ticket against the closure rubric — genuine resolution, classification, owner, time logged, title accuracy, client-facing closure message — then let it close or bounce it back with an itemized note.
category: QA & Closure
tools: [search_tickets, update_ticket, add_ticket_note, list_ticket_statuses, run_assistive_ai]
---

# Ticket QA Review

The quality gate between "tech marked it done" and "ticket actually closes." Grades the ticket against a fixed rubric, passes clean closures through to close (and CSAT where configured), and reopens failures with a plain-text per-criterion result the tech can act on.

## When to use

- A ticket reached completed / ready-to-close status and needs a quality check before it closes for good.
- "QA this ticket before I close it" / "did we actually resolve this?"
- A lead spot-checking a tech's closure on a specific ticket.
- Embedded in a Flow on status change (see Unattended variant below).

## Steps

1. Retrieve the full ticket with `search_tickets`: every note and message in order, time entries, board, type/subtype, priority, owner, title, and status. This thread is the single source of truth for every criterion below.
2. Grade each criterion PASS or FAIL, strictly from the thread:
   - **Genuine resolution.** Requires an explicit customer confirmation in the thread, or a technician note documenting a verbal/phone confirmation with who confirmed and when. A work summary is NOT customer confirmation — "I rebooted the server and it's working" describes work, it does not confirm the customer agrees. Dismissive or hostile replies ("fine", "whatever, just close it") do not count as confirmation; flag those for human review instead.
   - **Classification set.** Board, type/subtype, and priority are all populated and plausible for the issue described.
   - **Owner assigned.** A specific member owns the ticket — not unassigned, not a queue placeholder.
   - **Time logged.** At least one time entry exists and roughly covers the work described in the notes.
   - **Title accuracy.** The title describes the actual issue in plain terms (not "FW: help" or the raw alert subject). If wrong, suggest a corrected title; `run_assistive_ai` can draft one.
   - **Client-facing closure message.** The last client-visible message tells the customer what was done and that the ticket is closing. An internal-only wrap-up does not satisfy this.
3. **All criteria PASS** → allow closure: set the closed status with `update_ticket`, and where the desk has a CSAT survey configured, closing triggers it — do not suppress that.
4. **Any criterion FAILS** → perform BOTH steps, in this order, never one without the other:
   1. `update_ticket` to move the ticket back to its prior working status (reopen).
   2. `add_ticket_note` (internal) with the itemized result — plain text, no markdown or emojis (PSA sync compatibility). One line per criterion: `CRITERION: PASS` or `CRITERION: FAIL - <what is missing, one sentence>`. End with the single next action that would make it pass.
5. Report the outcome in one short line: passed and closed, or reopened with N failing criteria.

## Guardrails

- Score 0 (FAIL) if the evidence is not in the source of truth — never assume, never infer, never give credit for work that "was probably done." The thread is the record; if it is not written there, it did not happen for QA purposes.
- Never fabricate confirmations, ticket numbers, or links in the QA note.
- The reopen and the note travel together: never reopen silently, never post a failure note while leaving the ticket closed.
- Do not edit the tech's notes or rewrite history — the QA note is additive.
- Hostile or clearly unhappy final replies are not a QA pass or fail on their own; flag the ticket to a human lead rather than closing over an upset customer.
- If ticket write tools are disabled for this tenant, output the per-criterion result in chat and recommend the reopen instead of performing it.

## Unattended (Flows) variant

- Trigger: status change to completed / ready-to-close.
- Your entire reply is posted verbatim as the ticket note — no narration, no preamble, no markdown. Emit only the plain-text per-criterion PASS/FAIL block described in step 4.
- Deterministic behavior: all PASS → set closed status and stop. Any FAIL → reopen, then post the note, then stop.
- If the ticket is no longer in the trigger status when you read it, or the thread is ambiguous (e.g. confirmation may have arrived on another channel), do nothing and stop. A wrong reopen is worse than a missed one — when in doubt, do nothing.

## Consolidates

Closure QA gates, quality-assurance scoring (weighted and pass/fail), resolution compliance audits, completed-ticket confirmation checks, and forensic closure-review variants.
