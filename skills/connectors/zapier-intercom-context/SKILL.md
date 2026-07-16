---
name: Zapier Intercom Context
description: When a client runs their customer support on Intercom and their tooling problem lands on our desk — pull the relevant Intercom conversation context (read-focused) onto the ticket so the tech sees what the client's support team and end users actually experienced. Use for "check what their Intercom threads say", "their support tool is acting up — get the context", or enriching tickets about a client's Intercom-adjacent issues.
category: Connectors
tools: [search_tickets, search_contacts, add_ticket_note, "zapier: Intercom"]
---

# Zapier Intercom Context

When the client's own customer-support stack (which runs on Intercom) has a problem — widget down, replies not sending, a VIP end-customer thread gone wrong — the evidence lives in their Intercom, not in our ticket. This skill pulls the relevant conversation and user context out of the client's Intercom onto the Thread ticket, read-focused: we're borrowing their evidence, not working their support queue.

## When to use

- "<client> says their Intercom messenger is broken for customers — pull what their conversations show."
- A ticket about the client's support tooling needs the actual conversation evidence (timestamps, error behavior, affected users).
- "Find the conversation their ops lead mentioned and put the relevant part on the ticket."
- Verifying after a fix that new conversations are flowing again.

## Steps

1. **Verify before promising (Intercom's Zapier inventory is not pre-verified in this library):** run the Zapier Action Discovery pattern first — confirm find/lookup actions for conversations and users/contacts actually exist and are enabled. Intercom's Zapier surface has historically been trigger-heavy and lookup-thin; if no conversation-find action exists, be honest that context must come from the client exporting or screensharing, and stop at the degrade path.
2. Anchor the search from the ticket: which end user, company, or time window the issue concerns. Get identifiers (email, conversation link) from the ticket or the client contact — don't trawl their inbox speculatively.
3. Look up the conversation(s) and/or user records that match the anchor. Multiple candidates → list them for the member to pick; wrong-thread evidence is worse than none.
4. Extract only what the ticket needs: timeline of the failure (when messages stopped/bounced), error-adjacent content, affected user counts if visible — not the full transcript, and not the end customers' personal details beyond what diagnosing requires.
5. `add_ticket_note` (plain text): the extracted context with "per <client>'s Intercom, conversation <ID/link>, as of <date>" provenance, so a tech can click through under their own access if needed.
6. If the investigation produces something their support team must know (workaround, ETA), that goes back through the agreed human channel or an Intercom internal note **only if** the agreement covers writes and a note-write action verifiably exists — never as a reply to their end customer.
7. Post-fix verification on request: re-check that new conversations/replies are flowing in the affected window, and note the observation with timestamps.

## Guardrails

- **Member-connected Zapier required** (the client's Intercom workspace, standing client-granted access). Degrade: tell the member exactly what to ask the client for (conversation links, screenshots, export of the affected thread) and note on the ticket that Intercom context is pending.
- Read-focused stance: this skill pulls context. It never replies to the client's end customers, never assigns/closes their conversations, never edits their users — their support queue belongs to their team.
- Their end customers' data is third-party data: extract the minimum needed to diagnose, keep names/emails out of the note where a count or a pattern suffices, and never copy payment or sensitive personal content onto the ticket.
- Provenance on every extract: conversation ID/link and read date, so the evidence is checkable.
- No fabrication: if the lookup can't find the conversation, say so — do not reconstruct "what it probably said".
- Task cost: each Zapier call = 2 Zapier tasks — expect one or two lookups per context pull; scope searches tightly rather than paging through their inbox.
