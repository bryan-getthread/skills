---
name: View Builder
description: Create or duplicate an inbox view from a plain-English spec — "a view of my open P1s", "duplicate the dispatch view but filtered to <client>". Use when a member asks for a new view, a saved filter, or a variant of an existing view.
category: Automation & Flows
tools: [view_list, view_save, view_duplicate, view_getCurrent, view_listFilterAttributes, view_searchFilterValues]
---

# View Builder

Turn a plain-English description of "the tickets I want to see" into a saved inbox view — reusing an existing view where one is close, and mapping every filter to the tenant's real attribute values.

## When to use

- "Make me a view of unassigned tickets on the alerts board older than a day."
- "Duplicate the VIP view but only show tickets waiting on us."
- "I want a saved filter for my open P1s and P2s."

## Steps

1. Parse the spec into filter conditions, sort, and a short view name.
2. Call `view_list` first. If an existing view is close, propose `view_duplicate` + adjusted filters instead of building from scratch; if the member says "like my current view but…", start from `view_getCurrent`.
3. Map every condition through `view_listFilterAttributes` and `view_searchFilterValues` — exact value strings only, never guessed.
4. Show the recipe before saving: view name, each filter with its exact value, sort order. Confirm.
5. `view_save`. Report the saved view's name and restate what it shows, including what it deliberately excludes.

## Guardrails

- The views family is available to the in-app SuperAgent only. **Degradation:** when these tools are absent (external MCP), output the complete filter recipe — attribute, operator, exact value, sort — as a numbered list the member can click through in the Views UI in under a minute.
- Never overwrite or modify a shared/team view without explicit confirmation; prefer creating a personal copy.
- If a requested filter has no matching attribute, say so and offer the nearest real attribute — do not silently approximate.
- Name views descriptively and neutrally (no client nicknames or internal slang that won't translate across the team).
