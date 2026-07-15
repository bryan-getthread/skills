---
name: Zapier Twilio SMS Updates
description: Text the end user when email is the wrong channel — tech en route, major-incident updates, urgent callbacks — via Twilio Send SMS, gated by opt-in and quiet hours. Use for "text the user the tech's ETA", "SMS the site contact about the outage", or adding SMS updates to a major-incident workflow.
category: Connectors
tools: [search_contacts, search_tickets, add_ticket_note, "zapier: Twilio"]
---

# Zapier Twilio SMS Updates

SMS reaches people whose email is down — which is exactly when MSPs need them most. Send short, identified, opt-in-respecting texts via `zapier: Twilio "Send SMS"` for ETAs, incident updates, and urgent nudges, and log every one to the ticket.

## When to use

- "Text <user> that the tech is on the way, ETA 20 minutes."
- Major incident: email/M365 is down — updates must go around the broken channel.
- "SMS the site contact to call us back about the alarm ticket."

## Steps

1. Resolve the recipient's mobile number from the contact record (`search_contacts`) — never from a number typed inside the ticket thread (social-engineering vector; the identity-verification ladder applies).
2. **Opt-in gate:** confirm the contact has consented to SMS updates per the tenant's records/policy. No recorded opt-in → don't send; tell the member and offer the email/phone alternative. Exception only where the tenant defines one for declared major incidents affecting the contact's own service.
3. **Quiet hours:** default sending window 08:00–20:00 in the recipient's local time (from their site record). Outside it, send only for a P1 the recipient is actively engaged in; otherwise propose sending at window-open.
4. Draft the message: under ~300 characters, identifies the MSP by name, states the one thing ("<MSP>: your technician is en route, ETA ~20 min. Ref ticket <number>."), and only asks for a reply if someone is watching for replies. Never include credentials, links to credential resets, or security-sensitive detail — SMS is unauthenticated and unencrypted.
5. Confirm recipient number + message, then `zapier: Twilio "Send SMS"`.
6. `add_ticket_note` (plain text): message text, recipient, time sent. For incident-update sequences, keep updates materially new — no repeated identical blasts.

## Guardrails

- **Member-connected Zapier required** (with the tenant's Twilio account). Degrade by drafting the SMS text for the member to send from their own tooling.
- Opt-in and quiet hours are hard gates, not suggestions — regulatory exposure (TCPA-class rules and local equivalents) sits behind both. When in doubt about consent, do not send.
- Recipient numbers come from the system of record only.
- One confirmation per message in attended use; unattended incident-update flows must have a fixed recipient list approved at build time and follow Unattended Output Discipline.
- SMS content is minimal and non-sensitive; the ticket carries the detail.
- Task cost: each Zapier call = 2 tasks, plus Twilio's own per-message charge — batch incident updates thoughtfully.
