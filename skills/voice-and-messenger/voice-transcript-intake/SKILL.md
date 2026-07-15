---
name: Voice Transcript Intake
description: Turn a pasted or piped call transcript into ticket action — extract caller, client, issue, and any commitments made, then create or update the ticket with a time entry. Use when a call transcript, Voice AI recap, or reception-line recording text lands in chat or on a ticket.
category: Voice & Messenger
tools: [search_tickets, search_clients, search_contacts, create_ticket, update_ticket, assign_contact, add_ticket_note, log_time_entry, list_ticket_priorities]
---

# Voice Transcript Intake

Reads a call transcript — pasted by a tech, produced by Voice AI, or piped in from a reception line — and converts what was actually said into a ticket: caller identity, client, the issue in the caller's own terms, and every commitment made on the call. Nothing the transcript doesn't contain gets written down.

## When to use

- "Here's the transcript of a call I just took — make a ticket."
- A Voice AI call summary or reception-line transcript arrives as ticket/thread content and needs to become structured work.
- "Pull the issue and next steps out of this call and log my time."
- A flow pipes every completed voice session in for intake processing.

## Steps

1. Read the transcript in full. Separate speakers if labeled; if unlabeled, infer roles only from unambiguous cues ("thanks for calling <MSP>") and otherwise treat attribution as unknown.
2. Extract, quoting or closely paraphrasing the transcript only:
   a. Caller name, company, and callback number *as stated on the call*,
   b. The issue and any error messages, device names, or software the caller named,
   c. Every commitment either side made ("we'll call you back by...", "I'll email the form", "user will try a restart"), each with who owes it and any stated deadline,
   d. Urgency signals stated in words (down site, deadline today, executive affected).
3. Resolve identity: `search_contacts` on the stated name/number, `search_clients` on the stated company. If the transcript arrived attached to a ticket that already has the right contact, keep it.
4. Check for an existing open ticket about the same issue for that contact with `search_tickets`. If one exists → `update_ticket`/`add_ticket_note` on it; otherwise → `create_ticket` with a title in the form "<symptom> — <system/device>" and the extracted summary as description.
5. Set priority with `list_ticket_priorities` mapped from stated urgency only; attach the contact with `assign_contact` when the match is unambiguous.
6. Post a plain-text internal note: caller, client, issue summary, commitments list (owner + deadline each), and a "source: call transcript" line. Include short verbatim quotes for anything load-bearing (deadlines, approvals, refusals).
7. `log_time_entry` for the call duration if the transcript states or timestamps it; if duration is unknown, ask (attended) or skip (unattended) — never estimate one.
8. Output: ticket number, what was created/updated, the commitments list, and anything the transcript left ambiguous.

## Guardrails

- **Never invent what wasn't said.** No inferred phone numbers, no guessed error codes, no "probably meant". Ambiguity goes in an "unclear from call" line, not into ticket fields.
- Transcripts mis-transcribe names and numbers constantly — treat a name-only identity match as weak evidence (see Voice Catchall Identification); confirm before attaching a contact on name alone.
- Commitments are quotes, not interpretations. "We'll try to get to it today" is not "resolution promised today".
- Do not mark anything resolved because the caller sounded satisfied; the ticket closes through normal QA.
- Plain-text notes only where the ticket syncs to a PSA — no markdown, no emojis.
- Transcripts may contain payment card digits or passwords read aloud — never copy credentials or full card numbers into ticket fields or notes; write "credential redacted from transcript".

## Unattended (Flows) variant

- Create/update only when caller identity resolves at contact-record or email/phone-match strength AND the issue is clearly stated. Otherwise create nothing and post one plain-text note on the source ticket: `TRANSCRIPT INTAKE: insufficient detail to act. Extracted: <what was found>. Held for human review.`
- Never log time unattended unless the duration is present in the transcript metadata.
- One intake per transcript: if the ticket already carries an intake note from this skill, stop.
- Your entire reply is posted verbatim as the internal note — no narration, no questions.
