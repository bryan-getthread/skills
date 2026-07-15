---
name: Voicemail Follow-up Email
description: Draft the "called and left a voicemail" email — a short written trail after an unanswered call, with the callback number and ticket reference.
category: Communication
tools: [search_tickets, view_openDraft, add_ticket_note, log_time_entry]
---

# Voicemail Follow-up Email

The two-line email that pairs with an unanswered call: creates the written record, gives the client an async way back, and counts as a documented contact attempt.

## When to use

- "I called <user> and got voicemail — send the follow-up email."
- "Draft the 'tried to reach you' email for this ticket."
- Desk process requires every call attempt to be paired with an email.

## Steps

1. Confirm the essentials with the tech or ticket: who was called, roughly when, what the call was about, and the callback number the client should use (the desk's main line unless the tech specifies their direct line).
2. Draft — short by design, per the Email Baseline Standard (communication/email-baseline-standard):
   - "I just tried to reach you by phone about <issue> and left a voicemail."
   - What we need or what we wanted to tell them, in one sentence.
   - "You can reach us at <callback number>, or simply reply to this email — whichever is easier."
   - Reference the ticket number so the thread and the call tie together.
3. Offer the bookkeeping alongside the draft: a plain-text internal note via add_ticket_note recording the call attempt (date/time, voicemail left), and a time entry via log_time_entry if the desk logs call attempts — both only with the tech's confirmation.
4. Present the email via view_openDraft.

## Guardrails

- Only state that a call was made and voicemail left if the tech says so — never fabricate a call attempt to pad the contact record.
- Callback number must be provided or on file; never guess a phone number. If none is available, flag "NEEDS: callback number" instead of inventing one.
- Keep it to 3–4 sentences; this is a receipt, not the update itself. If there's substantive news, that's a normal client reply (communication/client-reply).
- This email counts as a documented attempt for the follow-up ladder (communication/followup-chaser) — the internal note is what makes it count, so recommend it every time.
- Internal notes in plain text (PSA compatibility). Zero-assumption rule on time entries: log only what the tech confirms happened.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft in chat labeled "DRAFT."
