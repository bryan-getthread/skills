---
name: Zapier Slack Escalation Ping
description: Ping the right engineer in Slack — DM or channel — with a one-line brief and ticket link when a ticket needs eyes now, with anti-nag dedupe so nobody gets the same ping twice. Use for "ping <member> about this ticket", "let the escalations channel know", or SLA-jeopardy nudges.
category: Connectors
tools: [search_tickets, search_members, add_ticket_note, "zapier: Slack"]
---

# Zapier Slack Escalation Ping

The fastest honest interrupt: one message via `zapier: Slack "Send Direct Message"` (or "Send Channel Message" to the desk's escalation channel) carrying exactly what the recipient needs to decide in five seconds — what, how urgent, which ticket — and a dedupe discipline so the desk's Slack doesn't become the boy who cried breach.

## When to use

- "Ping <member> — ticket <number> needs a senior look before the SLA blows."
- A triage or watch pass finds a ticket that must jump the queue.
- "Drop this in the escalations channel with the link."

## Steps

1. Resolve the recipient deliberately: the ticket owner, the designated escalation contact, or whoever the member names — via `search_members` and the tenant's escalation policy. For on-call rotation lookups, PagerDuty owns the truth (see Zapier PagerDuty On-Call); don't guess who's on call from Slack presence.
2. **Anti-nag dedupe:** check the ticket's notes for a prior escalation ping about the same condition. Already pinged and nothing material has changed → do not ping again; report that a ping is outstanding and since when. Re-ping only when the member explicitly asks or the situation has genuinely escalated (new severity, new deadline) — and say what changed.
3. Compose the one-liner: severity/urgency, one-sentence issue, client-safe identifier, ticket number and link, and the specific ask ("needs owner in 30 min", "approve the change"). One message, no essay — the ticket holds the detail.
4. Choose the surface: DM for a named individual; the escalation channel when the ask is "someone claim this". Show recipient + message for confirmation, then send via `zapier: Slack`.
5. Record it: `add_ticket_note` (plain text) — who was pinged, where, when, and the ask. This note is what step 2 reads next time.
6. If the ping demands a response and none arrives in the stated window, that's a fact to surface to the member — not a license to auto-ping hourly.

## Guardrails

- **Member-connected Zapier required** (Slack). Degrade: output the composed one-liner for the member to send, and still note the escalation attempt on the ticket.
- Dedupe is the product. A ping that repeats trains people to ignore pings; check the ticket record before every send.
- The ask must be decidable from the message alone — a bare "look at this?" is not an escalation.
- Urgency honesty: never inflate severity to get attention; the desk's escalation channel only works if severity words mean something.
- No client personal data or credentials in Slack messages; client-safe identifiers only.
- Task cost: each Zapier call = 2 tasks — one ping is one call; the dedupe check is a ticket read, not a Slack search.
