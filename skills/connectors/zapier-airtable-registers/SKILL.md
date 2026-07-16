---
name: Zapier Airtable Registers
description: Use a client-facing Airtable base as a shared register — append rows for assets, requests, or changes from tickets, and look up register entries to answer ticket questions. Append + lookup patterns only, against the agreed base and table. Use for "log this in the shared Airtable", "check the asset register", or clients who keep a register the MSP co-maintains.
category: Connectors
tools: [search_tickets, search_clients, add_ticket_note, "zapier: Airtable"]
---

# Zapier Airtable Registers

Some clients keep a shared Airtable as the register of record — assets, change log, request tracker — and the MSP's job is to keep it fed and to read it back. This skill does exactly two things against the agreed base: **append** a well-formed row when a ticket produces a register-worthy fact, and **look up** rows when a ticket needs what the register knows. It is not a general Airtable editor.

## When to use

- "Add the replacement laptop to <client>'s asset register in Airtable."
- "Log this change/request as a row in the shared base."
- "What does the register say about <device> / this request?" while working a ticket.
- A recurring pass appending resolved register-worthy tickets as rows.

## Steps

1. **Verify before promising (Airtable's Zapier inventory is not pre-verified in this library):** run the Zapier Action Discovery pattern first — confirm actions equivalent to "Create Record", "Find Record" (or a search action), and optionally "Update Record" exist and are enabled. If Find is missing, the lookup half of this skill is off the table — say so rather than promising reads that will be blind appends.
2. Confirm the target: base, table, and the field schema the client expects (field names, select-field option values, required columns). The schema is the client's — get it from the agreement or a one-time read, and reuse it; never invent field names or select options.
3. **Lookup pattern:** for register questions, find records by the natural key the register uses (asset tag, serial, request ID). Report matches with the fields that answer the question and "per the <client> register, as of <date>" provenance on the ticket via `add_ticket_note`. Zero matches → say the register has no entry; never fill the gap from memory.
4. **Append pattern — find before create:** before appending, search for an existing row with the same natural key. Found → this is an update conversation, not an append: show the member the existing row and the proposed change, and only update if "Update Record" is available and the agreement covers it. Not found → compose the new row.
5. Compose the row from ticket facts only: values matching the client's field schema exactly (their select options, their date format), a source/ticket-reference field if the schema has one. Show the composed row for confirmation, then create.
6. Close the loop: `add_ticket_note` (plain text) — what was appended or looked up, in which base/table, with the record ID.
7. Output ends with: rows appended vs matched-existing, lookups answered, and anything skipped because the schema didn't fit (fields the ticket couldn't fill honestly stay empty — never pad).

## Guardrails

- **Member-connected Zapier required** (the client's or tenant's Airtable base, standing access). Degrade: output the row as labeled field/value pairs for manual entry, note the pending append on the ticket, and answer lookups from ticket history with an explicit "register not consulted" caveat.
- Append + lookup only: this skill never deletes rows, never restructures tables or fields, never touches views. Schema changes belong to whoever owns the base.
- Their schema is law: field names and select options come from the client's table exactly. A creative select value forks their reporting.
- Register data is client-visible and often client-audited: factual values only, no internal commentary, no credentials in any field, no other clients' data.
- Empty is honest: a field the ticket doesn't evidence stays blank — a register padded with guesses is worse than a sparse one.
- Task cost: each Zapier call = 2 Zapier tasks — one find + one create per append, one find per lookup; batch periodic register passes and never poll the base for changes.
