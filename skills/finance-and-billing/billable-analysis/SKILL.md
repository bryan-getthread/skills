---
name: Billable Analysis
description: When someone asks how billable hours break down by technician, client, or period, or wants to see unbilled-work exposure from time entries.
category: Finance & Billing
tools: [search_tickets, search_members, search_clients]
---

# Billable Analysis

Analyze logged time entries to show billable vs unbillable hours by technician, client, or period, and quantify unbilled-work exposure — hours logged as billable that have not yet been invoiced.

## When to use

- "How many billable hours did <tech> log last month?"
- "Show billable vs unbillable by client for this quarter."
- "How much billable time is sitting uninvoiced right now?"
- "Which techs have the lowest billable ratio this period?"

## Steps

1. Confirm scope with the requester: period, and whether to slice by technician, client, or both. If no period given, default to the last full calendar month and say so.
2. Pull tickets with time entries for the period using `search_tickets`. Run one search per technician (from `search_members`) or per client (from `search_clients`) rather than one giant query, so result caps hit per-slice, not globally.
3. From each ticket's time entries, sum hours by billable flag. Time entries are the source of truth — do not infer billability from ticket type, board, or agreement name.
4. Compute per-slice: total hours, billable hours, unbillable hours, billable ratio. For unbilled exposure, list billable entries on tickets not yet marked invoiced/closed-billed (or, if invoicing status is not visible in Thread, report billable hours and state that invoiced-vs-not must be confirmed in the accounting system).
5. Output a plain table: slice | total hrs | billable | unbillable | ratio | unbilled exposure, followed by 2–3 observations (largest exposure, biggest ratio change if a prior period was compared). Flag which numbers are exact and which hit a search cap.

## Guardrails

- Time-entry evidence is the source of truth for billability analysis. Never reclassify an entry's billable flag in your math; report it as logged.
- If a search may have hit a result cap, say "at least N hours" — never present a capped total as exact.
- Do not state that unbilled hours "will be invoiced" or represent revenue owed; whether they are billable to the client depends on the agreement, which lives outside Thread. Label exposure as "logged-billable, invoicing status unconfirmed."
- Read-only: this skill changes nothing on tickets or time entries.
- Do not name-and-shame: when slicing by technician for a lead, present numbers neutrally and note that low billable ratios can reflect assignment mix, not effort.
