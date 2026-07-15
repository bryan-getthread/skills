---
name: Premature Confirmation Detector
description: Catch tickets closed on assumption — "should be working now", work summaries treated as confirmation, closures minutes after the last change — verify no customer confirmation exists, and reopen with a plain-text note asking for it.
category: QA & Closure
tools: [search_tickets, update_ticket, add_ticket_note, list_ticket_statuses]
---

# Premature Confirmation Detector

The single biggest reopen cause: closing because the tech believes it is fixed, not because the customer said so. This skill hunts the telltale closure patterns, checks the thread for actual confirmation, and reopens the ones closed on hope.

## When to use

- "Check today's closures for premature confirmations" / "did anyone close on assumption?"
- Focused follow-up after Reopen Forensics shows premature closure as the leading failure mode.
- Verifying one suspicious closure a client complained about.
- Embedded in a Flow on close (see Unattended variant) as a narrow, confirmation-only gate where the full QA rubric is too heavy.

## Steps

1. Scope: one ticket, or a window of closed tickets via `search_tickets` (split per board, disclose caps).
2. For each ticket, locate the confirmation evidence, if any. Valid confirmation is exactly one of:
   - An inbound client-visible message after the fix confirming the issue is resolved.
   - A technician note documenting a verbal confirmation: who confirmed, when, and how ("spoke with <user> at <time>, confirmed login working").
3. Flag the premature-closure tells when no valid confirmation exists:
   - Closure note says "should be fine / should be working / believe this is resolved."
   - The last thread event before close is the tech's own work summary — work performed is not outcome confirmed.
   - Closed within minutes of the last change with no client interaction in between.
   - The client's last actual message reports the problem still occurring, and the closure never addressed it.
   - Dismissive client reply ("fine", "whatever") treated as confirmation — it is not; it is a sentiment flag.
4. Verdict per ticket: **CONFIRMED** (valid evidence, cite the note/message and date), or **PREMATURE** (no valid confirmation — cite the tell).
5. For each PREMATURE ticket, on approval, perform both steps in order:
   1. `update_ticket` back to a working or waiting-confirmation status (map via `list_ticket_statuses`).
   2. `add_ticket_note` internal, plain text, no markdown: "Reopened - closed without customer confirmation. Last client message: <date, one-line gist>. Needed: client confirmation or a documented verbal confirmation. Please confirm with <user> and re-close." Never one step without the other.
6. Output: verdict list with evidence citations, reopened count, and — across a batch — which tells appeared most (feeds the Weekly QA Digest).

## Guardrails

- Evidence lives in the thread or it does not exist: never assume a confirmation happened off-channel. If the tech says it did, the fix is documenting it, not skipping QA.
- A work summary is never a confirmation, however detailed. The customer's outcome, not the technician's action, closes the loop.
- Reopen + note are inseparable; never reopen silently and never nag by note while leaving the ticket closed.
- Do not reopen tickets where the client cannot confirm by design (internal maintenance, no-contact alert tickets) — mark them N/A and note that a validation record replaces confirmation there.
- Batch reopens require explicit sign-off per ticket or per approved list.
- Notes are factual and blame-free; the pattern-level coaching belongs to the lead.

## Unattended (Flows) variant

- Trigger: status change to closed / completed.
- Your entire reply is the plain-text reopen note from step 5 — posted verbatim, no narration. The status change back happens by tool call first, then the note, then stop.
- Deterministic rules: reopen only when the thread shows NO valid confirmation AND at least one explicit tell from step 3. If confirmation is arguable (an inbound message that might mean resolved, a note that might document a call), do nothing and stop — a false reopen erodes trust in the gate. N/A ticket classes (alert boards, internal maintenance) are excluded by the Flow filter or by you; when unsure whether the class applies, do nothing.
