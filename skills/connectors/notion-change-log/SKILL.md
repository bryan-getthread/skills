---
name: Notion Change Log
description: Append approved changes to a queryable change-log database in Notion — what changed, for which client, who approved it, and the source ticket. Use when asked to "log this change", "record the approved change in Notion", or "what changed for <client> last month".
category: Connectors
tools: [search_tickets, send_approval]
connectors: [Notion]
---

# Notion Change Log

**When to use:** "Log the firewall change from ticket <number> to the change log," or "query the change log: what changed for <client> in the last 30 days?"

## Prompt

```
Maintain an append-only, queryable record of approved changes across clients in Notion, so
"what changed around the time this broke?" has an answer that isn't archaeology.

This needs the member's connected Notion (notion-search, notion-create-pages,
notion-create-database, notion-query-data-sources). If Notion isn't connected, degrade by
outputting the formatted entry for manual paste and point the member to connect Notion — do
not stop.

1. Locate the database. notion-search for the change-log database. If none exists, propose
   creating one (notion-create-database) with properties: Date, Client, System, Change summary,
   Approved by, Source ticket, Rollback notes — confirm before creating.
2. Verify the approval before logging. Pull the source ticket (search_tickets) and confirm
   approval evidence exists: a completed send_approval outcome, an explicit written
   client/manager approval in the thread, or a signed authorization. No evidence → do not log;
   report what's missing. Only approved AND executed changes get logged — never log a planned or
   recommended change as done.
3. Draft the entry: date, client, system touched, one-paragraph factual change summary, approver
   (role + where the approval lives, e.g. "approved in ticket <number>" — evidence, never
   inferred), and rollback notes if the ticket has them. No credentials or secret values in
   summaries ("rotated the <system> admin credential", not the credential).
4. Show me the entry, then notion-create-pages into the database. Append only — never edit or
   delete past entries; corrections go in as new entries referencing the original.
5. Read side: for "what changed" questions, notion-query-data-sources filtered by
   client/system/date range and return matching entries with page links. (SQL-backed queries
   need a Business+ workspace with Notion AI; degrade to notion-search + database views.)
```
