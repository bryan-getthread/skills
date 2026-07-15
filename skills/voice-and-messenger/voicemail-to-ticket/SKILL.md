---
name: Voicemail to Ticket
description: Convert a voicemail transcription into a ticket with a callback commitment — urgency read from what the caller said, not from tone guesses. Use when a voicemail lands as ticket/thread content or is pasted in.
category: Voice & Messenger
tools: [search_contacts, search_clients, search_tickets, create_ticket, update_ticket, assign_contact, add_ticket_note, list_ticket_priorities]
---

# Voicemail to Ticket

Turns a voicemail transcription into an actionable ticket: who called, what they need, what number to call back, and a callback owed — with urgency set only from the words in the message.

## When to use

- A voicemail transcription arrives on the intake board or is pasted into chat.
- "Left on the after-hours line: <transcription> — make a ticket."
- A flow routes every voicemail-sourced ticket through structuring.
- Pair with Voicemail Follow-up Email (communication) when the desk also sends a written trail after the callback attempt.

## Steps

1. Read the full transcription. Extract only what is present: caller name, company, callback number as spoken, the request, and any stated time constraint ("before we open at 8", "I'm leaving at noon").
2. Transcribed phone numbers are error-prone: keep the number exactly as transcribed and flag it as unverified if it doesn't match the contact record found in step 3.
3. Resolve identity: `search_contacts` on stated name/number, `search_clients` on stated company. Check `search_tickets` for an open ticket this voicemail continues ("calling about my ticket from yesterday") — update that ticket instead of creating a duplicate.
4. Set urgency from content only, via `list_ticket_priorities`:
   - Stated business stoppage, safety issue, security concern, or "everyone is down" → high.
   - Stated deadline today → elevated.
   - Everything else → the desk's default intake priority.
   Never raise priority because the caller "sounded" angry or stressed — transcripts carry no reliable tone, and guessing writes fiction into the queue.
5. `create_ticket` (or `update_ticket` on the existing one): title "<request/symptom> — voicemail from <caller>", description = cleaned transcription plus extracted fields. `assign_contact` only on an unambiguous match.
6. Record the callback commitment in a plain-text note: number to call, best-by time if the caller stated one, otherwise the desk's standard callback SLA. This is the line Voice Commitment Tracker sweeps later, so keep the format consistent: `CALLBACK OWED: <number> by <time/SLA>`.
7. Output: ticket number, priority + the stated words that justified it, callback line, and any unverified fields.

## Guardrails

- Urgency evidence must be quotable. If you cannot point at the words that make it urgent, it is not urgent.
- Never invent a callback number; if none was left and no contact record supplies one, say so on the ticket — that itself changes the callback plan (email instead).
- Garbled or partial transcriptions: capture what exists, mark gaps as "[inaudible in transcription]", and do not fill them.
- Voicemails sometimes contain credentials or card numbers read aloud — redact them from ticket fields and notes.
- Plain-text notes only for PSA-synced tickets.

## Unattended (Flows) variant

- Create a ticket only when there is a resolvable client (contact/company match, or the source line is dedicated to one client). Unresolvable → post one plain-text note on the source: `VOICEMAIL INTAKE: caller/client unresolved. Heard: <extracted fields>. Held for human review.` and stop.
- Priority above default requires a quoted urgency phrase in the note; otherwise use the default.
- Never mark the callback as made — this skill records the debt, it does not pay it.
- One ticket per voicemail; if an intake note from this skill already exists, stop.
- Your entire reply is posted verbatim as the internal note — no narration.
