---
name: Notion Client Runbook Database
description: Create and maintain a client-runbooks database in Notion — one entry per client per system — and update entries when a ticket reveals an environment fact has changed. Use when asked to "set up a client runbook database", "update <client>'s runbook", or "keep our client docs in Notion current".
category: Connectors
tools: [search_tickets, search_clients]
connectors: [Notion]
---

# Notion Client Runbook Database

**When to use:** "Create a Notion database for our client runbooks," or "ticket <number> shows <client> migrated their file server — update their runbook."

## Prompt

```
Stand up (once) and keep current a structured client-runbooks database in Notion:
environment facts, key systems, procedures, and owners per client — sourced from ticket
evidence, not memory.

This needs the member's connected Notion (notion-search, notion-fetch, notion-create-database,
notion-update-data-source, notion-create-pages, notion-update-page, notion-query-data-sources).
If Notion isn't connected, tell the member to connect it in settings and degrade by outputting
the entry content and schema for manual entry — do not stop.

1. Locate or create. notion-search for an existing client-runbooks database; only if none
   exists, propose notion-create-database with properties: Client, System/Area, Owner, Last
   verified (date), Status, Category — confirm schema and teamspace before creating. One
   database is the goal; a second runbooks database is a bug. Extend an existing schema
   (notion-update-data-source) only with confirmation.
2. Populate an entry. For a given client (search_clients for canonical naming), draft one page
   per system/area with facts each traceable to a source ("from ticket <number>", "confirmed
   by <member role>"). Facts require a source — "probably" facts are recorded as "Unverified —
   <source>" or left out. No credentials in entries; reference the credential vault entry name.
3. Update on change. When a ticket reveals a changed environment fact: notion-query-data-sources
   (or notion-search) for the affected entry, notion-fetch its current content, show me a
   before/after diff of the specific fact, and only on confirmation notion-update-page —
   updating the fact, the Last verified date, and a one-line change note with the source ticket
   reference. Never overwrite a fact without showing the diff; a wrong runbook is worse than a
   stale one because it's trusted.
4. Report what was created or changed, with page links. Keep end-user personal data out of entries.
```
