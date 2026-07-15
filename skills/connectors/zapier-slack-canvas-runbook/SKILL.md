---
name: Zapier Slack Canvas Runbook
description: Maintain a live incident canvas in Slack during a major — running timeline, current owners, workstreams, and the next-update time — updated in place so there is one source of truth instead of a scrolling channel. Use for "start an incident canvas", "update the canvas", or major-incident comms hygiene.
category: Connectors
tools: [search_tickets, add_ticket_note, "zapier: Slack"]
---

# Zapier Slack Canvas Runbook

Channels scroll; canvases hold. During a major incident, keep one Slack canvas as the living state of the incident — timeline, owners, impact, next update time — edited in place via the Slack canvas actions, so a responder joining at hour three reads one document instead of three hundred messages.

## When to use

- A major incident has a swarm channel (see Zapier Slack Swarm Channel) and needs a single evolving status document.
- "Start the incident canvas for the <client> outage" / "update the canvas — mitigation in progress, next update 15:30."
- Post-incident: freezing the canvas as the timeline input to the post-mortem.

## Steps

1. Create once, in the incident's channel: one canvas per incident, seeded with the standard skeleton — **Summary** (one line), **Impact** (who/what, client-safe), **Status** (Investigating / Identified / Mitigating / Monitoring / Resolved), **Owners** (incident lead, comms, workstreams), **Timeline** (append-only, timestamped, with timezone stated), **Next update at** (an explicit time, always).
2. Record the canvas reference on the ticket: `add_ticket_note` (plain text) so the canvas is findable from the ticket and vice versa.
3. On every update, **edit the existing canvas** — never create a second one. Timeline entries are appended with a timestamp and author; Status and Next-update-at are overwritten to current truth. History lives in the timeline, not in duplicate canvases.
4. Timeline entries record facts, not hopes: what was observed, what was done, what changed. "Probably fixed" is not a timeline entry; "change deployed 14:42, error rate dropping" is.
5. "Next update at" is a promise — when an update lands, set the next one. If there is genuinely nothing new at the promised time, the update is "no change, next update at <time>". A stale next-update time is how stakeholders lose trust.
6. On resolution: set Status to Resolved, append the closing timeline entry, copy the final summary + timeline onto the ticket via `add_ticket_note`, and leave the canvas intact in the (soon-archived) channel as the post-mortem record.

## Guardrails

- **Member-connected Zapier required** (Slack workspace where canvases are enabled — verify on first use; not all plans/workspaces have them). Degrade: maintain the same skeleton as a pinned ticket note updated in place, and say the canvas isn't available.
- One incident, one canvas, edited in place. Duplicates fork the truth.
- All times carry a timezone; incident timelines cross zones and "3pm" is a bug.
- The canvas is readable by everyone in the channel — client-safe wording, no credentials, no blame. Names of responders as owners: yes; speculation about fault: no.
- Never mark Resolved on the canvas before the member confirms resolution on the ticket — the ticket is the system of record; the canvas mirrors it.
- Task cost: each Zapier call = 2 tasks — batch canvas edits into meaningful updates (status change, timeline milestone), not per-message mirroring of channel chatter.
