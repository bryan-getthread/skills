---
name: Zapier Slack Swarm Channel
description: Spin up a dedicated Slack channel for an incident or high-touch client issue — create it, invite the right people, post the ticket brief, and archive it with a resolution summary on close. Use for "open a swarm channel for this", "get everyone in a channel for the <client> outage", or major-incident channel hygiene.
category: Connectors
tools: [search_tickets, search_members, add_ticket_note, update_ticket, "zapier: Slack"]
---

# Zapier Slack Swarm Channel

One incident, one channel, one record: create a per-incident Slack channel via `zapier: Slack "Create Channel"`, pull in exactly the people who should be there, seed it with a ticket brief so nobody asks "context?", and close it out properly — resolution summary posted, channel archived, everything mirrored to the ticket.

## When to use

- A major incident or multi-client outage needs live coordination beyond ticket notes.
- "Open a swarm channel for ticket <number> and get the on-call and the account lead in."
- A high-severity, high-touch client issue where the desk swarms rather than queues.

## Steps

1. Qualify first: swarm channels are for genuinely multi-person, active incidents. A routine ticket gets a ping (see Zapier Slack Escalation Ping), not a channel. If in doubt, ask the member.
2. Dedupe before creating: search for an existing channel for this incident/ticket (naming convention below) and check the ticket's notes for a channel reference. An existing channel gets reused — a second channel splits the conversation and is the failure this skill exists to prevent.
3. Name by convention: `inc-<ticket-number>-<short-slug>` (lowercase, hyphenated). Consistent names are how step 2 works next time.
4. Create via `zapier: Slack "Create Channel"`, then invite the resolved roster: ticket owner and named responders via `search_members`, plus whoever the member designates (on-call, account lead). Resolve people from records and the member's explicit list — never invite by guessing who "usually handles this".
5. Post the ticket brief as the channel's first message: ticket number, client placeholder-safe summary, current impact, what's been tried, and who owns what. This message is the channel's context anchor.
6. Mirror to the ticket: `add_ticket_note` (plain text) with the channel name and when it was opened, so anyone on the ticket can find the swarm.
7. During the incident, offer the canvas companion (see Zapier Slack Canvas Runbook) for timeline/owners rather than pinning ever-growing messages.
8. On close: post a resolution summary to the channel (what happened, fix, follow-ups), copy that summary onto the ticket via `add_ticket_note`, confirm with the member, then archive the channel via `zapier: Slack "Archive Channel"`. An unarchived dead channel is clutter; an archived one is a searchable record.

## Guardrails

- **Member-connected Zapier required** (Slack workspace with channel-create rights). Degrade: post the ticket brief and roster as text for the member to create the channel manually, and still note the intended channel name on the ticket.
- One incident, one channel — dedupe check is mandatory before every create.
- The channel is internal by default; if the client is invited (shared/Connect channels), everything posted is client-facing text — say so in the brief and keep internal commentary on the ticket.
- Never archive without the resolution summary posted and mirrored to the ticket; the archive is the last step, not the first.
- No credentials or secrets in channel messages, ever — Slack history outlives the incident.
- Task cost: each Zapier call = 2 tasks. Create + invites + brief is the expected shape (~3–4 calls); don't post routine status chatter through Zapier — that's for humans in the channel.
