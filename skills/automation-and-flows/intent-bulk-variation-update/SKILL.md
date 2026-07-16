---
name: Intent Bulk Variation Update
description: Apply a shared argument or reply block across all of an intent's client variations at once, with a per-variation diff preview and explicit confirmation before any write. Answers the commonly requested "change this everywhere without editing each client by hand" workflow; runs manually on demand.
category: Automation & Flows
tools: [get_intent, set_variation_arguments, set_variation_replies, update_variation]
---

# Intent Bulk Variation Update

When an intent has many per-client variations and you need the same change in all of them — a new argument, an updated reply block — editing each by hand is slow and error-prone. Apply it across every variation in one pass, but only after showing exactly what changes where.

## When to use

- "Add this argument to every variation of the <intent>"
- "Update the reply block across all client variations at once"
- "Roll this change out to every variation, not just one"
- Standardizing a shared change while keeping client-specific bits intact

## Steps

1. Load the intent and all its variations with get_intent. Identify which variations are in scope (all of them, or a named subset).
2. Build the change set: the exact argument(s) or reply block to apply, and whether it *adds to* or *replaces* what each variation currently has. Default to additive unless the member says replace.
3. **Diff preview, per variation.** For each in-scope variation, show current → proposed so client-specific content that would be overwritten is visible. Call out any variation where the change would clobber something distinct, so the member can exclude it.
4. Wait for explicit confirmation on the previewed diff. Let the member exclude specific variations before proceeding.
5. On confirm, apply the change variation by variation via set_variation_arguments / set_variation_replies (or update_variation), tracking each result.
6. Report a summary: which variations were updated, which were skipped/excluded, and any that failed. Re-preview before re-running on failures.

## Guardrails

- Confirm before writing, always — bulk edits touch many client-facing configs at once, so the diff preview and an explicit yes are mandatory.
- Preserve client-specific content: flag (don't silently overwrite) any variation whose existing arguments/replies are distinct from the shared block.
- Additive by default; only replace when the member explicitly says so.
- Report partial success honestly — list exactly which variations changed and which didn't.
- No fabrication of variations or content; act only on what get_intent returns.
- Member-invoked only; no unattended variant (Flows can't schedule bulk config edits).
