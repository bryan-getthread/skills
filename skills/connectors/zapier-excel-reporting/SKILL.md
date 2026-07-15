---
name: Zapier Excel Reporting
description: Append rows to client report workbooks in Excel (Microsoft 365) and read license-count or register sheets during procurement and ticket work. Use for "add this month's numbers to the client's workbook", "check the license sheet", or register lookups when the tracker is Excel rather than Google Sheets.
category: Connectors
tools: [search_tickets, search_clients, add_ticket_note, "zapier: Microsoft Excel"]
---

# Zapier Excel Reporting

The Excel twin of Zapier Google Sheets Reporting: the MSP↔client trackers that live in Microsoft 365 workbooks — SLA reports, license counts, asset registers — written append-only via `zapier: Microsoft Excel "Add Row"` and read mid-ticket via "Find Row" / "Get Range". A shared workbook is a ledger with formulas attached; this skill adds rows and never breaks the machinery.

## When to use

- "Append June's ticket volume and SLA attainment to <client>'s report workbook."
- During procurement: "how many <vendor> licenses does the register workbook say <client> holds?"
- "Is <device> on their asset sheet?" — when the register is an Excel file.

## Steps

**Appending report rows:**
1. Resolve the exact workbook and worksheet from the tenant's convention or the member — never guess between similarly named files; a row in the wrong client's workbook is a data leak.
2. Read the header/table layout first ("Get Range" on the header row, or "Find Row" against the table) and map values to the existing columns. Rows go into the existing **table** where one exists — Excel formulas and pivots hang off tables, and an out-of-table row silently vanishes from the client's charts.
3. Values come from tool results (the desk's own counts and SLA figures) — no estimates written as fact; a capped search result gets reported as capped, not written.
4. **Dedupe:** "Find Row" for the period/key about to be written. Exists → stop and show it; changing an existing row ("Update Row") is a member decision, not a default.
5. Show the composed row against the headers for confirmation, then `zapier: Microsoft Excel "Add Row"`. `add_ticket_note` (plain text) when the write is ticket-driven.

**Register lookups:**
6. "Find Row" keyed on the register's key column (SKU, user, device); "Get Range" for count blocks. Report with provenance — "per the license workbook" — and flag staleness: a workbook is only as current as its last editor. For procurement decisions, recommend cross-checking against the live source (e.g. the M365 admin portal) before purchase.

## Guardrails

- **Member-connected Zapier required** (Microsoft account with access to the workbook, typically in SharePoint/OneDrive). Degrade: output the formatted row for manual paste, or answer lookups from other sources stating the workbook wasn't checked.
- Append-only posture: add rows; never clear ranges, delete rows, rewrite headers, or touch formula columns. "Clear" actions exist in the app — this skill does not use them.
- Wrong-workbook is the failure to fear: confirm file + worksheet + client binding before the first write of a session.
- The workbook is client-visible: values and text written are client-facing.
- License counts from a spreadsheet are claims, not inventory truth — say so whenever the number drives a purchase.
- Task cost: each Zapier call = 2 tasks — one find + one add per row; for multi-row reports, one batch confirmation then per-row writes.
