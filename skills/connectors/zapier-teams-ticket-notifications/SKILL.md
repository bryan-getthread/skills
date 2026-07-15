---
name: Zapier Teams Ticket Notifications
description: Post ticket updates and escalations into Microsoft Teams channels via Zapier — a client's shared channel, the internal escalations channel, or a per-board feed. Use for "notify the Teams channel when this escalates", "post this update to the client's channel", or wiring Teams alerts into a flow.
category: Connectors
tools: [search_tickets, add_ticket_note, "zapier: Microsoft Teams"]
---

# Zapier Teams Ticket Notifications

Put ticket news where people already look: post updates, escalations, and P1 alerts into Teams channels using `zapier: Microsoft Teams "Send Channel Message"` (or "Send Chat Message" for a person) — with the channel locked down so a mis-fill can't broadcast to the wrong audience.

## When to use

- "When ticket <number> escalates, post to the internal escalations channel."
- "Send the major-incident update to <client>'s shared Teams channel."
- A flow needs a Teams notification action for P1s on a board.

## Steps

1. Identify audience and channel. Internal channel vs client-facing shared channel changes the message's entire register — confirm which before drafting.
2. Draft the message: ticket reference, one-line status, what happens next, who owns it. Client-facing: plain professional language, no internal jargon, no other clients or internal tech chatter. Internal: include priority, client, and the deep context a responder needs.
3. Show the message and named destination channel for confirmation.
4. Send via `zapier: Microsoft Teams "Send Channel Message"` (channel), "Send Chat Message" (individual), or a message card action where the tenant's curated server exposes it.
5. `add_ticket_note` (internal, plain text) recording that the notification went out and where.
6. For recurring wiring (a flow that always posts P1s), hand the trigger/filter design to Flow Builder and keep this skill as the action spec.

## Guardrails

- **Member-connected Zapier required:** these actions exist only if the member (or admin) has connected Zapier with a Teams-authenticated account. If absent, say so, point them to connect Zapier, and output the drafted message for manual posting.
- **Curated-server, locked-channel guidance:** for anything recurring or unattended, the admin should pin the exact Teams action on a curated Zapier server with the channel field locked. A locked channel makes "posted a client's incident into another client's channel" structurally impossible — the worst failure mode of this skill.
- Confirm the channel every time when it isn't locked; never resolve a channel by name similarity.
- Cross-tenant hygiene: a client's channel gets only that client's information, ever.
- Task cost: each successful Zapier call = 2 Zapier tasks. Notify on state changes, not on every note — a chatty flow burns the task budget and trains people to ignore the channel.
- No sensitive data (credentials, security detail) in Teams messages; reference the ticket instead.
