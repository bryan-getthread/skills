---
name: Zapier Freshdesk Cross-File
description: For co-managed clients whose internal helpdesk runs Freshdesk — cross-file tickets into their Freshdesk, exchange reference numbers both ways, and relay material updates in each direction. Use for "file this in their Freshdesk", "link our ticket to their FD number", or co-managed desks with Freshdesk on the client side.
category: Connectors
tools: [search_tickets, add_ticket_note, update_ticket, "zapier: Freshdesk"]
---

# Zapier Freshdesk Cross-File

The Freshdesk sibling of the Zendesk cross-file: the client's internal IT runs Freshdesk, incidents must exist in both systems, and each ticket carries the other's number. Reference-exchange conventions follow the Comanaged Reference Exchange skill; this skill is the Freshdesk-specific transport, minding Freshdesk's own vocabulary (reply vs private note, groups, ticket fields, statuses).

## When to use

- "<client>'s IT team needs this in their Freshdesk — cross-file ticket <number>."
- The client hands over a Freshdesk ticket number to link with ours.
- "Check their Freshdesk ticket and bring our ticket up to date."
- Co-managed desks where the client's Freshdesk is their system of record.

## Steps

1. **Verify before promising (Freshdesk's Zapier inventory is not pre-verified in this library):** run the Zapier Action Discovery pattern first — confirm actions equivalent to "Create Ticket", "Add Note/Reply to Ticket", "Update Ticket", and a "Find Ticket" lookup exist and are enabled, and check whether note privacy (private note vs public reply) is controllable. If privacy isn't a settable field, every relayed update must be written as requester-visible.
2. Confirm the agreement's shape: their group/type/priority conventions, required ticket fields, the requester to file under, and which updates flow in which direction (per the Comanaged Reference Exchange conventions).
3. **Find before create:** search their Freshdesk for a ticket already carrying our reference or the client-supplied number. Found → link to it; a duplicate on their desk breaks their SLA reporting.
4. Create the ticket: subject in their convention, client-safe description carrying the Thread ticket reference, field values from the client's specification. Show the composed ticket for confirmation, then create.
5. **Exchange references both ways:** capture their ticket ID; `add_ticket_note` (plain text) on the Thread ticket with their number, filed/linked date, and the agreed update flow; ensure our number is on their record. Both tickets carrying both numbers is the completion condition.
6. Updates outbound: material Thread-side progress → private note on their ticket where privacy is controllable and the agreement says staff-only; public reply only where the client wants their requester updated through their system. Client-safe, dated, ticket-referenced.
7. Updates inbound: on request or per pass, read their ticket and relay material changes (status, group, agent notes of substance) onto the Thread ticket via `add_ticket_note` with "per Freshdesk ticket <number>" provenance.
8. Closure: their resolve/close and our resolution are separate events under separate rules — mirror each as a note on the other; never resolve their ticket unless the agreement explicitly grants it.

## Guardrails

- **Member-connected Zapier required** (the client's Freshdesk instance, standing client-granted access). Degrade: output the composed ticket/note text for someone with Freshdesk access, and flag on the Thread ticket that cross-filing and the reference exchange are pending.
- Their helpdesk: touch only what the cross-filing agreement covers — never their other tickets, automations, or contact records.
- Reply-vs-private-note is Freshdesk's sharp edge: a reply emails their requester. When privacy can't be verified for a write, compose the text as safe for the end user to receive.
- Their taxonomy (groups, statuses, custom fields) comes from the client's specification, never from guessing.
- Everything written is client-side record: professional, no MSP-internal commentary, no credentials, no other clients' data.
- Task cost: each Zapier call = 2 Zapier tasks — one find + one create per cross-file, one call per relay; material-change sync only, no polling.
