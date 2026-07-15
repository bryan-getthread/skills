---
name: Notion Intake Forms
description: Build a Notion form view for structured requests (new hires, access requests, project intake) and convert submitted rows into tickets — structured intake without another form vendor. Use when asked to "make a request form", "set up new-hire intake in Notion", or "turn form submissions into tickets".
category: Connectors
tools: [notion-search, notion-create-database, notion-create-view, notion-update-view, notion-query-data-sources, create_ticket, search_clients, search_contacts]
---

# Notion Intake Forms

Two halves of one loop: a Notion form view that captures a request with every field the desk needs up front, and a processing pass that turns submitted rows into correctly-populated tickets — exactly once each.

## When to use

- "We keep getting half-complete new-hire requests — build an intake form."
- "Set up a Notion form for access requests that lands as tickets."
- "Process this week's form submissions into tickets." (the processing half)

## Steps

**Building the form (one-time):**
1. `notion-search` for an existing intake database for this request type; extend rather than duplicate.
2. Design the database schema from what the desk needs to act without a follow-up question — e.g. new hire: name, start date, manager, role/template user, applications, hardware, client/site. Add two processing properties: **Processed** (checkbox) and **Ticket ref** (text).
3. Confirm schema + teamspace, then `notion-create-database` and `notion-create-view` with type `form`. Share the form link with the member to distribute.

**Processing submissions (repeatable):**
4. `notion-query-data-sources` for rows where Processed is false.
5. For each row: validate required fields (incomplete → skip and list it for human follow-up, don't guess); resolve the client (`search_clients`) and requester (`search_contacts`); draft the ticket (title in the desk's standard format, description carrying every form field, correct board).
6. Show the batch of draft tickets for confirmation, then `create_ticket` for each and write back Processed = true and the ticket number to the row (`notion-update-page` on the row).
7. Report: created tickets with numbers, skipped rows with reasons.

## Guardrails

- **Member-authenticated connector:** requires the member's Notion connection; degrade by designing the schema + form fields as a spec the member builds by hand, and note tickets must then be created from submissions manually or via chat.
- The Processed flag is the dedupe mechanism — never create a ticket from a row without setting it, and never process a row that has it. Double tickets erode trust in the whole loop.
- Never fabricate missing form data; a row missing required fields becomes a follow-up task, not a guessed ticket.
- Form-submitted personal data goes into the ticket, not into chat summaries beyond what's needed.
- Batch confirmation before creating tickets; unattended variants of this loop must follow Unattended Output Discipline and skip (not guess) on every ambiguity.
