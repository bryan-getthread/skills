# Contributing

Thanks for adding to the library. A good Skill is small, does one job, and is safe to run.

## What makes a Skill worth merging

- **One workflow.** If it needs the word "and" twice to describe, it is probably two Skills.
- **A real trigger.** The `description` is what the agent matches on. Write it as *when to use this*, not as a feature summary.
- **Guardrails.** Anything that writes, closes, deletes, sends, or touches credentials needs an explicit guardrail. State when to confirm with a human and what never to do.
- **No private data.** No client names, hostnames, credentials, ticket IDs, partner names, or environment-specific board and status names. Use generic placeholders and let the person adapt.

## Format

Add a folder under the right category in `skills/` with a single `SKILL.md`:

```
---
name: Short Title
description: When to use this, in one line.
category: One of the existing categories
tools: [tool_one, tool_two]
---

# Short Title

**When to use:** ...

**What you get:** ...

## Steps
1. ...

## Guardrails
- ...

## Consolidates
If this merges known variants, name them here.
```

Keep steps to a handful. If a step needs a paragraph, the Skill is doing too much.

## Merging variants

If your Skill overlaps an existing one, extend the existing one rather than adding a near-duplicate. Fold the extra capability into its steps and note it under `Consolidates`. The point of this library is a small set of strong Skills, not a long tail of near-copies.

## Review

Open a pull request. A maintainer will check the trigger wording, the guardrails, and that nothing private slipped in.
