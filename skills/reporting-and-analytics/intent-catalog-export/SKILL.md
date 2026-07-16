---
name: Intent Catalog Export
description: Compile the desk's full intent catalog (every intent and its detail) and export it to Excel or Google Sheets via Zapier, so non-admins can review intents without admin access — read-only. Answers the commonly requested "give me the whole intent list in a spreadsheet" workflow; runs manually on demand.
category: Reporting & Analytics
tools: [list_intents, get_intent]
---

# Intent Catalog Export

Reviewing the desk's intents shouldn't require an admin seat. Compile every intent into one catalog and push it to a spreadsheet so anyone can read, sort, and comment on it — without touching the configuration.

## When to use

- "Export all our intents to a spreadsheet"
- "Give me the full intent catalog in Excel / Google Sheets"
- "I need to review our intents but I'm not an admin"
- Preparing an intent audit, cleanup, or roadmap review offline

## Steps

1. List every intent with list_intents, then get_intent for each to pull the detail worth reviewing: name, description/purpose, whether it's enabled, variation count, and (where exposed) key arguments/replies at a glance.
2. Assemble a flat catalog — one row per intent — with consistent columns so it sorts and filters cleanly in a spreadsheet.
3. **Export via Zapier.** Confirm the member has connected Zapier and has an authenticated Google Sheets / Excel action (see the zapier-action-discovery gate). Then push the catalog with `zapier: Google Sheets "Create Spreadsheet Row(s)"` or `zapier: Microsoft Excel "Add Row(s)"`. If Zapier/those actions aren't set up, deliver the catalog as an in-chat table instead and say the export path needs Zapier connected.
4. Report the row count and, if exported, the destination — and note the export reflects the catalog at this moment (re-run to refresh).

## Guardrails

- **Read-only.** This skill lists and reads intents; it never creates, edits, enables, or deletes one. Say so explicitly in the output.
- **Admin-token note:** listing/reading full intent detail may require an admin-scoped Thread token. If the member's token can't read intents, say that plainly rather than exporting a partial catalog silently.
- Zapier export depends on member setup — verify the connection and the specific Sheets/Excel action exist before promising the export; degrade to an in-chat table otherwise.
- No fabrication — every row comes from a real intent; don't invent intents or fill blank fields with guesses.
- Note that the export is a point-in-time snapshot.
- Member-invoked only; no unattended variant (Flows can't schedule an export).
