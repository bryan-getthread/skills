---
name: Zapier ServiceNow Cross-File
description: Cross-file a ticket into an enterprise client's ServiceNow — create the incident/request record via table CRUD, track the sys_id on the Thread ticket, and relay updates both ways. Use for "file this in their ServiceNow", co-managed enterprise clients, or "check what their SNOW record says now".
category: Connectors
tools: [search_tickets, add_ticket_note, update_ticket, "zapier: ServiceNow"]
---

# Zapier ServiceNow Cross-File

Enterprise and co-managed clients often require every incident to exist in *their* ServiceNow too. Zapier's ServiceNow app is table-level record CRUD + find — no magic ITSM verbs — so this skill works the tables directly: find-before-create on the right table (usually `incident`), capture the returned **sys_id** as the permanent handle, and keep both records pointing at each other.

## When to use

- "<client> requires everything filed in their ServiceNow — cross-file ticket <number>."
- Co-managed desks where the enterprise's SNOW is the client-side system of record.
- "Pull the current state of their SNOW incident onto our ticket."

## Steps

1. Confirm the target: which table (`incident` unless the client's process says request/change) and the client's required fields (caller, category, assignment group values are *their* taxonomy — use the values the client has specified; never invent them).
2. **Find before create:** search the table for an existing record carrying this ticket's reference (their correlation/reference field, or description text). Found → update/comment that record; a second record for the same incident breaks the client's reporting.
3. Create the record via ServiceNow record-create on the agreed table: short description in the client's convention, full client-safe description with the Thread ticket reference, and the client-specified field values. Show the composed record for confirmation first.
4. **Capture the sys_id:** the create result's sys_id is the record's permanent key. `add_ticket_note` (plain text): table, record number (INC…), **sys_id**, and when filed. Every future read/update targets the sys_id — numbers are for humans, sys_ids are for tools.
5. Updates outbound: material ticket progress → update the SNOW record (work notes / state per the client's process) addressed by sys_id, client-safe text, ticket-referenced.
6. Updates inbound: on request or per pass, fetch the record by sys_id and relay material changes (state, assignment, work notes) onto the ticket via `add_ticket_note` with "per ServiceNow <number>" provenance.
7. Closure: their record closing and the Thread ticket resolving are separate events in separate processes; mirror each into the other as a note, but each system closes by its own rules.

## Guardrails

- **Member-connected Zapier required** (the client's ServiceNow instance — standing, client-granted access). Degrade: output the record payload as formatted text for someone with instance access, and note on the ticket that cross-filing is pending.
- The sys_id-on-ticket rule is non-negotiable; without it, every later sync is a fragile text search in someone else's instance.
- It is the client's instance: create and update only what the cross-filing agreement covers. Never touch other tables, other records, or their assignment/workflow data beyond the agreed fields.
- Their taxonomy, their values: category, priority, assignment group come from the client's specification — a guessed assignment group routes their incident to nowhere.
- Everything written is client-visible enterprise record: plain, professional, no MSP-internal commentary, no credentials.
- Task cost: each Zapier call = 2 tasks — one find + one create per cross-file, one read per sync pass; don't poll.
