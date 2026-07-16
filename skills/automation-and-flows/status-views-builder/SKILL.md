---
name: Status Views Builder
description: Generate one saved inbox View per ticket status ("Waiting on Customer", "Waiting on Approval", …) scoped to the member's boards, so the inbox can be filtered by status in one click. Answers the commonly requested "let me filter my inbox by status" workflow.
category: Automation & Flows
tools: [list_ticket_statuses, list_boards, view_list, view_save, view_listFilterAttributes, view_searchFilterValues]
---

# Status Views Builder

Build the full set of per-status saved Views in one pass — one View per ticket status the member cares about, each scoped to their boards — instead of hand-creating them one at a time. Sibling of `automation-and-flows/view-builder`: that skill builds a single view from a plain-English spec; this one fans out the specific per-status set partners keep asking for.

## When to use

- "I want to filter my inbox by status" / "give me a view for every status".
- A member wants "Waiting on Customer", "Waiting on Approval", "In Progress" etc. as one-click tabs.
- A desk lead wants a standard status-view set rolled out for a board.

## Steps

1. Enumerate statuses with `list_ticket_statuses` and boards with `list_boards`. Confirm with the member which statuses they actually want views for (default: the waiting/held statuses first — those are the ones people filter for) and which boards to scope to.
2. Check `view_list` for existing views whose names or filters already cover a target status — never create a duplicate; report "already exists" instead.
3. For each remaining status, resolve the correct filter attribute via `view_listFilterAttributes` and the exact status value via `view_searchFilterValues` (status names vary per board — do not hardcode).
4. Create each view with `view_save`: filter = status match + the member's boards; name it plainly after the status (e.g. "Waiting on Customer"). Keep names well under the 256-character limit.
5. Output a summary table: view name, status filter, boards, and created/skipped-as-duplicate for each.

## Guardrails

- View names must be ≤ 256 characters — enforce before saving.
- Always check `view_list` first; creating a second "Waiting on Customer" view is worse than none.
- Create only what was confirmed — don't blanket-generate a view for every status on every board unless asked; closed/cancelled statuses are usually noise.
- Views are member-scoped saved filters, not shared configuration — say so if the requester expects the whole team to see them.
- **Degradation:** the views family (`view_list`, `view_save`, …) is in-app SuperAgent only. On an external MCP surface, do not pretend to create views — output the filter recipe for each status (attribute, value, boards) so the member can save them manually in the inbox, and say that is what you are doing.

## Unattended note

This skill is attended-only: view creation needs the member's confirmation of statuses and boards. Do not embed it in a Flow.
