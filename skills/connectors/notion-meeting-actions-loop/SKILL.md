---
name: Notion Meeting Actions Loop
description: Run the recurring client-meeting cycle in Notion — pull this meeting's action items into tickets, then build the next meeting's status page showing what happened to last meeting's items. Use for "run the <client> meeting loop", "prep the status page for next week's sync", or any standing meeting whose action items keep evaporating between sessions.
category: Connectors
tools: [notion-query-meeting-notes, notion-search, notion-fetch, notion-create-pages, notion-update-page, notion-create-comment, create_ticket, search_tickets, search_clients, search_members]
---

# Notion Meeting Actions Loop

Meeting Notes to Tickets handles one meeting; this is its recurring-cadence big sibling. For a standing client sync (weekly ops call, monthly service review), each run closes the full loop: extract this meeting's commitments into tickets, then check what actually happened to *last* meeting's tickets and write the answer into a status page the next meeting opens with. The loop is what turns "we discussed this three weeks ago" into "item 4, resolved Tuesday, ticket <number>".

## When to use

- "Run the meeting loop for yesterday's <client> ops sync."
- "Prep the status page for next week's service review — where did last month's action items land?"
- Any recurring meeting where action items historically vanish between sessions.

## Steps

1. Find the latest meeting's notes: `notion-query-meeting-notes` for the named recurring meeting/date, or `notion-search` + `notion-fetch` for a given page. Confirm it is the right meeting before extracting.
2. **Forward half — commitments to tickets** (the Meeting Notes to Tickets discipline, applied verbatim): extract only explicit action items; resolve client (`search_clients`), contact, owner (`search_members`); dedupe against open tickets from prior runs of this same loop — a recurring meeting *re-discussing* an item is the classic duplicate trap, so check for the existing ticket before proposing a new one; present the full proposed list; one explicit confirmation; `create_ticket`; backlink each item to its ticket via `notion-create-comment` on the meeting page.
3. **Backward half — status of prior commitments:** collect the ticket numbers from previous meetings in this series (the backlink comments from step 2 of earlier runs make this findable), then `search_tickets` for each: status, resolution, owner, last activity. Report reality, including the unflattering parts — an item open for three meetings running gets said plainly, with age.
4. **Build the next-meeting status page:** create (`notion-create-pages`) or update (`notion-update-page`) the series' status page per the tenant's convention — one page updated in place, or a dated child page per meeting. Contents: **Done since last meeting** (item, ticket, resolution date), **In progress** (item, ticket, owner, current state), **Stalled/aging** (item, age, what it's waiting on), **New this meeting** (from step 2). Every line carries its ticket number — the page is a view of the tickets, not a second source of truth.
5. Output the run summary: tickets created, items skipped and why, prior-item statuses, the status page link, and anything needing a human call (unowned items, items discussed but not committed).

## Guardrails

- Notion tools are member-authenticated — the member must have connected Notion, and `notion-query-meeting-notes` needs a Notion plan with Notion AI. Degrade: ask for pasted notes and run the same loop from the paste, skipping page writes but still delivering the ticket work and status text.
- Confirm before creating tickets — one confirmation per batch, never silent creation; the dedupe-against-prior-runs check is mandatory, since recurring meetings re-litigate old items.
- Zero-assumption on commitments: only items the notes explicitly assign or agree to; no inferred owners, dates, or scope.
- The status page states only what the tickets show — no editorializing progress that isn't in the record, and no invented ticket references.
- The page is often client-visible: client-safe wording, no internal commentary, no blame framing on stalled items — age and blocker, stated flat.
- Update the existing status page/series structure; don't fork a new page hierarchy each run and orphan the old one.
