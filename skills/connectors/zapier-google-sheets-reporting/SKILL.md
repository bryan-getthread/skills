---
name: Zapier Google Sheets Reporting
description: Append SLA/report rows to client-facing Google Sheets and look up license or asset registers mid-ticket. Use for "add this month's numbers to the client's SLA sheet", "check the license register", or "what does the asset sheet say about <device>" when the register lives in Sheets.
category: Connectors
tools: [search_tickets, search_clients, add_ticket_note, "zapier: Google Sheets"]
---

# Zapier Google Sheets Reporting

Many MSP↔client rituals live in a shared spreadsheet: monthly SLA numbers, license counts, asset registers. This skill writes to those sheets append-only via `zapier: Google Sheets "Create Spreadsheet Row"`, and reads them mid-ticket via "Lookup Spreadsheet Row" — treating the sheet as a ledger, never a scratchpad.

## When to use

- "Append June's ticket volume and SLA attainment to <client>'s report sheet."
- Mid-ticket: "how many <vendor> licenses does <client> have on the register?" / "is <device> on the asset sheet?"
- A recurring reporting pass that maintains a running client-facing tracker.

## Steps

**Appending report rows:**
1. Resolve the exact spreadsheet and worksheet from the tenant's convention or the member — never guess between similarly named sheets; a row in the wrong client's sheet is a data leak.
2. Read the header row first (Lookup/Get) to learn the actual column layout; map values to the existing columns. Never invent new columns or reorder — the sheet's consumers (client, formulas, charts) depend on its shape.
3. Values come from tool results (ticket counts, SLA figures from the desk's own queries) — no estimated numbers written as fact. If a figure comes from a capped search, don't write it; report the cap instead.
4. **Dedupe:** `zapier: Google Sheets "Lookup Spreadsheet Row"` for the period/key about to be written. Row already exists → stop and show it; updating an existing row is a member decision ("Update Spreadsheet Row"), not a default.
5. Show the composed row against the sheet's headers for confirmation, then `zapier: Google Sheets "Create Spreadsheet Row"`. `add_ticket_note` (plain text) when the write is ticket-driven.

**Mid-ticket lookups:**
6. "Lookup Spreadsheet Row" keyed on the register's key column (user, device, license SKU). Report what the sheet says **as of the sheet** — a register is only as current as its last editor; say so when staleness matters (e.g. procurement decisions), and cross-check against a live source when one exists.

## Guardrails

- **Member-connected Zapier required** (Google account with access to the target sheets). Degrade: output the formatted row for manual paste, or answer lookups from other sources with the caveat that the register wasn't checked.
- Append-only posture: create rows; never clear ranges, delete rows, or touch formula columns. A client-facing sheet broken by an automated write costs more than it saves.
- Wrong-sheet is the failure to fear: confirm spreadsheet + worksheet + client binding before the first write of a session.
- The sheet is client-visible: values and any notes written are client-facing text.
- Register answers carry provenance: "per the license sheet (last relevant row dated <date>)" — never present a spreadsheet cell as live inventory truth.
- Task cost: each Zapier call = 2 tasks — one lookup + one write per row; for multi-row reports, confirm the batch once and write per-row without re-confirming each.
