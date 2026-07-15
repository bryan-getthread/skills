---
name: Call Summary Email
description: Draft the post-call recap email — what was discussed, what was agreed, and the actions each side owns — so a phone conversation becomes a written record.
category: Communication
tools: [search_tickets, view_openDraft, add_ticket_note, log_time_entry]
---

# Call Summary Email

Turns "per our call" into an actual record: a recap the client can confirm, with the action items split unambiguously between their side and ours.

## When to use

- "Just got off a call with <user> — draft the recap email."
- "Summarize this call for the client" — with pasted notes or a transcript.
- Desk process requires phone agreements to be confirmed in writing.

## Steps

1. Get the source material: the tech's notes, a pasted transcript, or a dictated rundown. The recap contains only what this source establishes — the agent wasn't on the call.
2. Draft per the Email Baseline Standard skill (communication/email-baseline-standard):
   - One line: "Thanks for the call today — here's a quick summary so we're aligned."
   - **What we discussed:** 1–3 sentences on the topics covered.
   - **What was agreed:** the decisions, stated as decisions.
   - **Actions — ours:** each with an owner-side framing ("We will <action> by <time>"). Include commitments only as stated in the notes.
   - **Actions — yours:** what the client agreed to do, phrased as a helpful reminder, not an order.
   - Close with the correction invitation: "If I've missed or misstated anything, just reply and let me know."
3. If the notes are ambiguous on any commitment (who owns it, or by when), ask the tech before drafting rather than guessing — a recap that assigns the wrong owner creates the dispute it was meant to prevent.
4. Offer the bookkeeping: plain-text internal note of the call via add_ticket_note, and a time entry via log_time_entry for the call duration if the tech confirms it.
5. Present the email via view_openDraft.

## Guardrails

- The recap contains only what the notes/transcript establish. Never add technical detail, commitments, or timelines from the ticket that weren't part of the call.
- Zero-assumption rule: a recommendation discussed on the call is written as discussed ("we talked about possibly upgrading X"), never as decided or done.
- Dates and owners on action items must be explicit in the source or confirmed by the tech.
- If a transcript was pasted, treat it as potentially containing sensitive tangents — recap the business content only; never quote small talk or third-party mentions.
- Internal notes in plain text (PSA compatibility); time entries only for confirmed durations.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft in chat labeled "DRAFT."
