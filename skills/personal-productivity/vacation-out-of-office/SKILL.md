---
name: Vacation Out-of-Office
description: Handle inbound activity on an away technician's tickets — post an external note with the return date and helpdesk contact, skip duplicates within 24h, and never touch status, priority, or assignment.
category: Personal Productivity
tools: [search_tickets, add_ticket_note]
---

# Vacation Out-of-Office

The OOO responder for tickets: when a client writes into an away technician's ticket, post one courteous external note with the return date and how to reach the helpdesk meanwhile. Deliberately minimal — it informs, and changes nothing else.

## When to use

- "I'm out from <date> to <date> — handle OOO replies on my tickets"
- Set up as an unattended Flow on client-response events while a tech is away
- A lead activating coverage messaging for an absent team member
- One-off: "post an OOO note on my open tickets before I leave"

## Steps

1. Collect the parameters: the away member, last working day, return date, and the helpdesk contact line for urgent needs (main queue email/phone or "reply URGENT in this thread" per the team's convention). Confirm the exact wording of the return date — this note goes to clients.
2. Determine the trigger set: in attended mode, the member's open tickets where a client has written since the absence began (search_tickets); in a Flow, the single ticket that fired the event.
3. Before posting on any ticket, check the ticket's recent notes: if an OOO note was already posted on this ticket within the last 24 hours, skip it — clients who write three times get one OOO note per day, not three.
4. Post one external (client-visible) note per qualifying ticket via add_ticket_note, plain text, no markdown or emojis (PSA compatibility), in this shape:
   - Thank them for the message
   - The assigned technician is out of office, returning <return date>
   - For anything urgent in the meantime: <helpdesk contact line>
   - Their ticket has not been forgotten and will be picked up on return
5. Keep the note short (4–5 sentences) and warm but unambiguous. If the desk operates in another language, write the note in the ticket's thread language.
6. Report what was done: tickets noted, tickets skipped as duplicates, tickets skipped for any other reason.

## Guardrails

- NEVER change ticket status, priority, or assignment — the OOO note is the entire action. Escalation and reassignment are humans' calls (or a separate, deliberate coverage process).
- Skip duplicates: max one OOO note per ticket per 24 hours, no exceptions.
- External note only when the inbound was from the client; never post an OOO note in response to internal notes or system/alert activity — a monitoring alert doesn't need a vacation reply.
- Use only the return date and contact line the member provided; never guess a return date. Do not share why the member is away or any personal detail.
- If the return date has passed (member is back but the Flow still runs), do nothing and flag that the automation should be disabled.

## Unattended (Flows) variant

- Your entire reply is the external note posted verbatim — no narration, no headers, no signature beyond the team's convention.
- Fire only when: the inbound is from the client, the ticket is assigned to the away member, and no OOO note exists in the past 24 hours. If any check fails or is uncertain, output nothing — when in doubt, do nothing.
- Never include markdown, emojis, or placeholders in the posted note; if a required parameter (return date, contact line) is missing, output nothing rather than a note with a blank.
