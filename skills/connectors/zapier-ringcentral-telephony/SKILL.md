---
name: Zapier RingCentral Telephony
description: For RingCentral shops — pull call-log context onto tickets ("did they call us / did we call them") and send SMS from the tenant's RingCentral number under the desk's SMS etiquette rules (opt-in, quiet hours, ticket channel first). Use for "check the call log for <contact>", "text them from our RingCentral number", or telephony-evidence questions on a ticket.
category: Connectors
tools: [search_tickets, search_contacts, add_ticket_note, "zapier: RingCentral"]
---

# Zapier RingCentral Telephony

For tenants whose phone system is RingCentral, two ticket-shaped needs: **call-log context** (when a dispute or timeline question turns on "who called whom, when") and **SMS from the tenant's number** for the narrow cases where a text is the right channel. SMS behavior — opt-in, quiet hours, content discipline — is inherited wholesale from the SMS Channel Etiquette skill; this skill only supplies the RingCentral transport.

## When to use

- "Did <contact> actually call us Tuesday? Pull the call log onto the ticket."
- A ticket timeline needs telephony evidence (missed calls, call times, durations).
- "Text <contact> from our number that the tech is on the way" — where SMS etiquette allows it.
- Confirming an outbound call happened before closing a "we'll call you" loop.

## Steps

1. **Verify before promising (RingCentral's Zapier inventory is not pre-verified in this library):** run the Zapier Action Discovery pattern first — confirm a "Send SMS" action and some call-log find/lookup action exist and are enabled. RingCentral's Zapier surface may be trigger-heavy (new-call/new-SMS triggers) with thin on-demand lookups; if no call-log search action exists, the context half degrades to asking someone with RingCentral portal access — say so honestly rather than promising log pulls.
2. **Call-log context:** anchor the lookup from the ticket — contact's number (via `search_contacts`), direction, and a tight time window. Pull matching entries and `add_ticket_note` (plain text): calls found with direction, timestamps, duration, and "per RingCentral log, as of <date>" provenance. Zero matches → note that too; "no record of the call" is itself evidence. Never paraphrase logs from memory.
3. **SMS — inherit the etiquette first:** apply the SMS Channel Etiquette rules before composing anything — verified opt-in for SMS contact, quiet-hours check against the recipient's timezone, and SMS reserved for short time-sensitive notices (arrival, outage update, callback confirmation), not support conversations.
4. Compose the SMS: short, plain, one purpose, identifies the MSP, references the ticket context without jargon. Show the member the exact text and recipient number for confirmation.
5. Send via the verified SMS action from the tenant's designated RingCentral number. `add_ticket_note` (plain text): message sent, recipient, timestamp, and the text itself — SMS off-ticket must still be on the record.
6. Replies: if the tenant's inbound SMS routes to the desk, note that; if it doesn't (send-only setup), the message must not invite a reply ("reply to this text" to a dead number strands the client) — include the right callback path instead.

## Guardrails

- **Member-connected Zapier required** (the tenant's RingCentral account). Degrade: for context, tell the member exactly what to pull from the RingCentral portal (number, window, direction); for SMS, hand the composed text to the member to send from a RingCentral client — and still log whatever was sent on the ticket.
- SMS etiquette is inherited, not optional: no opt-in on record → no text; inside quiet hours → hold or use another channel; conversation-shaped content → ticket channel, not SMS.
- Call logs are evidence: report exactly what the log shows with provenance, never reconstruct or infer calls that aren't in it.
- Phone numbers are personal data: numbers live in the contact record and the note's send-record — don't broadcast them into other systems.
- Send from the tenant's designated number only; never from a member's personal line through the integration.
- One-purpose texts, no marketing: broadcast notices belong to the Mailchimp Notices skill and consented audiences, not SMS blasts.
- Task cost: each Zapier call = 2 Zapier tasks — one lookup per context pull, one send per SMS; tight time windows on log searches, no polling for replies.
