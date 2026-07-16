---
name: Zapier Zendesk Cross-File
description: For co-managed clients whose internal helpdesk runs Zendesk — cross-file tickets into their Zendesk, carry each side's reference number on the other, and relay material updates both ways. Use for "file this in their Zendesk", "their ticket 4821 is ours too — link them", or co-managed desks with a Zendesk on the client side.
category: Connectors
tools: [search_tickets, add_ticket_note, update_ticket, "zapier: Zendesk"]
---

# Zapier Zendesk Cross-File

Co-managed clients with an internal Zendesk need incidents to exist in both systems, each carrying the other's reference — the same discipline as the ServiceNow cross-file, in Zendesk's vocabulary (tickets, internal notes vs public comments, their groups and forms). The mechanics of *which* references go where follow the Comanaged Reference Exchange skill; this skill is the Zendesk-specific transport.

## When to use

- "<client> requires everything in their Zendesk too — cross-file ticket <number>."
- The client's IT lead hands over their Zendesk ticket number: "this is ours as well."
- "Pull the current state of their Zendesk ticket onto ours."
- Co-managed desks where the client's Zendesk is their side's system of record.

## Steps

1. **Verify before promising (Zendesk's Zapier inventory is not pre-verified in this library):** run the Zapier Action Discovery pattern first — confirm actions equivalent to "Create Ticket", "Update Ticket" / "Add Comment to Ticket", and a "Find Ticket" lookup exist and are enabled, and check whether comment-privacy (public vs internal note) is a controllable field. That one field decides whether relayed updates can be kept staff-only; if it can't be set, treat every relayed comment as end-user-visible.
2. Confirm the cross-filing agreement's shape: which group/form/type on their side, which fields the client wants populated, and which direction each kind of update flows (per the Comanaged Reference Exchange conventions).
3. **Find before create:** search their Zendesk for a ticket already carrying our reference (or the client-supplied number, when they filed first). Found → link, don't duplicate: proceed to step 5 with the existing ticket.
4. Create the ticket: subject in the client's convention, client-safe description with the Thread ticket reference in the body (and their reference field, if the form has one), requester per the agreement. Show the composed ticket for confirmation, then create.
5. **Exchange references both ways:** capture their ticket ID and `add_ticket_note` (plain text) on the Thread side — their number, when filed/linked, and the agreed update flow. On their side, ensure our ticket number is on the record. Two tickets that don't carry each other's numbers will drift apart within a day.
6. Updates outbound: material Thread-side progress → comment on their ticket (internal note where privacy is controllable and the agreement says staff-only; public comment only where the client wants their end user updated through their system). Client-safe, dated, ticket-referenced.
7. Updates inbound: on request or per pass, read their ticket and relay material changes (status, assignee group, substantive comments) onto the Thread ticket via `add_ticket_note` with "per Zendesk ticket <number>" provenance.
8. Closure: each system closes by its own rules — their solve and our resolve are separate events; mirror each into the other as a note, and never solve their ticket unless the agreement explicitly hands the MSP that right.

## Guardrails

- **Member-connected Zapier required** (the client's Zendesk instance, standing client-granted access). Degrade: output the composed ticket/comment as formatted text for someone with Zendesk access, and note on the Thread ticket that cross-filing is pending with the reference exchange incomplete.
- Their helpdesk: create and update only what the cross-filing agreement covers. Never touch their other tickets, macros, triggers, or user records.
- Public-vs-internal comment discipline is the sharpest edge in Zendesk — when privacy can't be verified for a write, compose it as safe for the end user to read, because they might.
- Their taxonomy (groups, forms, priorities, custom fields) comes from the client's specification; a guessed group routes their ticket to nowhere.
- Everything written is client-side record: professional wording, no MSP-internal commentary, no credentials, no other clients' data.
- Reference-exchange completeness is the success condition: both tickets carry both numbers, verified, before this skill reports done.
- Task cost: each Zapier call = 2 Zapier tasks — one find + one create per cross-file, one call per relay; sync on material change, never poll.
