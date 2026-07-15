---
name: Notion Client Runbook Database
description: Create and maintain a client-runbooks database in Notion — one entry per client per system — and update entries when a ticket reveals an environment fact has changed. Use when asked to "set up a client runbook database", "update <client>'s runbook", or "keep our client docs in Notion current".
category: Connectors
tools: [notion-search, notion-fetch, notion-create-database, notion-update-data-source, notion-create-pages, notion-update-page, notion-query-data-sources, search_tickets, search_clients]
---

# Notion Client Runbook Database

Stand up (once) and keep current (continuously) a structured client-runbooks database: environment facts, key systems, procedures, and owners per client — updated from ticket evidence rather than memory.

## When to use

- "Create a Notion database for our client runbooks."
- "Ticket <number> shows <client> migrated their file server — update their runbook."
- Periodic "make sure the runbook DB matches reality" maintenance.

## Steps

1. **Locate or create.** `notion-search` for an existing client-runbooks database; only if none exists, propose `notion-create-database` with properties: Client, System/Area, Owner, Last verified (date), Status, and Category — confirm schema and teamspace location before creating. Extend an existing database's schema (`notion-update-data-source`) only with confirmation.
2. **Populate an entry.** For a given client (`search_clients` for canonical naming), draft one page per system/area with facts sourced from tickets and the member — each fact traceable to its source ("from ticket <number>", "confirmed by <member role>").
3. **Update on change.** When a ticket reveals a changed environment fact: `notion-query-data-sources` (or `notion-search`) for the affected entry, `notion-fetch` its current content, show a before/after diff of the specific fact, and on confirmation `notion-update-page` — updating the fact, the Last verified date, and a one-line change note with the source ticket reference.
4. Report what was created or changed, with page links.

## Guardrails

- **Member-authenticated connector:** the member must have connected Notion; without it, output the entry content and schema for manual entry and tell them to connect Notion in settings.
- No credentials in runbook entries — reference the credential vault's entry name only.
- Never overwrite an environment fact without showing the before/after diff and getting confirmation; a wrong runbook is worse than a stale one because it's trusted.
- Facts require a source; "probably" facts are recorded as "Unverified — <source>" or left out.
- One database is the goal — always search before creating; a second runbooks database is a bug.
- Client names in this database are internal-facing; keep end-user personal data out of entries.
