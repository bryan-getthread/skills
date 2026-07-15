---
name: Notion Change Log
description: Append approved changes to a queryable change-log database in Notion — what changed, for which client, who approved it, and the source ticket. Use when asked to "log this change", "record the approved change in Notion", or "what changed for <client> last month".
category: Connectors
tools: [notion-search, notion-create-pages, notion-query-data-sources, notion-create-database, search_tickets, send_approval]
---

# Notion Change Log

An append-only, queryable record of approved changes across clients — firewall rules, admin grants, config changes, migrations — so "what changed around the time this broke?" has an answer that isn't archaeology.

## When to use

- "Log the firewall change from ticket <number> to the change log."
- A change-management flow finishes its approval and the change needs recording.
- "Query the change log: what changed for <client> in the last 30 days?" (read side)

## Steps

1. **Locate the database.** `notion-search` for the change-log database. If none exists, propose creating one (`notion-create-database`) with properties: Date, Client, System, Change summary, Approved by, Source ticket, Rollback notes — confirm before creating.
2. **Verify the approval before logging.** Pull the source ticket (`search_tickets`) and confirm approval evidence exists: a completed `send_approval` outcome, an explicit written client/manager approval in the thread, or a signed authorization. No evidence → do not log; report what's missing instead.
3. Draft the entry: date, client, system touched, one-paragraph change summary in plain factual language, approver (role + where the approval lives, e.g. "approved in ticket <number>"), and rollback notes if the ticket contains them.
4. Show the entry, then `notion-create-pages` into the database. Append only — never edit or delete past entries; corrections go in as new entries referencing the original.
5. **Read side:** for "what changed" questions, `notion-query-data-sources` filtered by client/system/date range and return the matching entries with page links.

## Guardrails

- **Member-authenticated connector:** requires the member's Notion connection; degrade by outputting the formatted entry for manual paste and pointing the member to connect Notion.
- Only *approved and executed* changes get logged. Never log a planned or recommended change as done — the zero-assumption rule applies doubly to an audit trail.
- The approver field records evidence, not inference: name the artifact ("approval in ticket <number>"), never assume who approved.
- Append-only is a hard rule; an editable change log is worthless in a dispute.
- No credentials or secret values in change summaries ("rotated the <system> admin credential" — not the credential itself).
- SQL-backed `notion-query-data-sources` needs a Business+ workspace with Notion AI; degrade to `notion-search` + database views if unavailable.
