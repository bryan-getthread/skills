---
name: Closure Note Completeness
description: Check a ticket's closure note against the house standard before the close sticks — issue, cause, actions, outcome, confirmation reference — and draft the compliant version when it falls short.
category: QA & Closure
tools: [search_tickets, add_ticket_note, update_ticket, run_assistive_ai]
---

# Closure Note Completeness

The closure note is what the desk knows in six months: the next tech's starting point, the client's record, the billing narrative. This skill grades the note against the house standard and, unlike a pure gate, drafts the missing pieces from the thread so compliance costs seconds.

## When to use

- "Check my closure note" / "is this ticket documented well enough to close?"
- The documentation-focused companion to Ticket QA Review when only the note quality is in question.
- Bulk-checking a batch of pending closures before EOD.
- Writing the closure note properly in the first place: "draft a compliant closure note for this ticket."

## Steps

1. Retrieve the ticket with `search_tickets`: full thread, time entries, classification, and the current closure/resolution note.
2. Grade the closure note against the house standard. Default standard (override with the desk's own where provided):
   - **Issue** — what was wrong, in the client's terms, one or two lines.
   - **Cause** — what turned out to be behind it (or "root cause not determined" stated honestly).
   - **Actions** — what was actually done, specific enough to repeat or reverse.
   - **Outcome** — the state at close, in observable terms ("user logging in successfully"), not intentions ("should be fine").
   - **Confirmation reference** — who confirmed resolution and how, pointing at the confirmation in the thread (or the validation record for no-contact ticket classes).
   - **Follow-ups** — anything deferred, scheduled, or recommended, or explicitly "none."
3. Score each element PRESENT or MISSING, strictly from what the note says — an element buried in mid-thread notes but absent from the closure note is MISSING (the closure note must stand alone).
4. If everything is PRESENT: report pass, one line per element, and let the close stick.
5. If anything is MISSING: draft the compliant closure note from evidence already in the thread — `run_assistive_ai` can produce the base summary/recap, then structure it to the standard. Every sentence in the draft must trace to a thread entry; where the thread itself lacks the fact (no cause anywhere, no confirmation), leave an explicit placeholder like "CONFIRMATION: not yet obtained - confirm with <user> before closing" rather than papering over the gap.
6. Present the draft. On approval, post it with `add_ticket_note` (plain text, no markdown or emojis — this note syncs to the PSA and is often client-visible in exports). Where the desk's process requires it and the confirmation element is genuinely missing, recommend holding the close (`update_ticket` back to ready-to-close) until it exists.
7. For batches, output a compliance table (ticket, missing elements) and offer drafts ticket by ticket.

## Guardrails

- The draft documents what happened; it never invents what should have happened. A gap in the thread becomes a placeholder, not fiction — no fabricated causes, confirmations, or outcomes.
- "Should be working" and other intention phrasings are not an Outcome; require observable state or an honest "unverified."
- Do not overwrite or edit the tech's existing note; post the compliant version as an additional note and say so.
- Plain text only in posted notes (PSA compatibility); keep the language simple and localizable — closure notes get read by non-native speakers and clients.
- Posting and any status hold require explicit approval; grading alone is read-only.
- The completeness check gates documentation only — resolution truth (was it actually fixed and confirmed) belongs to Ticket QA Review and the Premature Confirmation Detector; run those for the full gate.
