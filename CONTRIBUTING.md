# Contributing

Thanks for adding to the library. The bar here is **value that works** — a small set of
strong, runnable skills beats a long tail of near-copies or prompts that can't actually
execute. Every skill is a copy-paste prompt an MSP service desk runs in Super Magic.

## Add or update a skill in 4 steps

1. **Copy the template.** Start from [`TEMPLATE.md`](TEMPLATE.md) into
   `skills/<category>/<slug>/SKILL.md` (kebab-case slug, one of the categories in the
   [README](README.md)).
2. **Check the tools.** Every tool your prompt uses must be in
   [`research/tool-catalog.md`](research/tool-catalog.md). If it needs an integration, list
   it under `connectors:` and make the prompt degrade gracefully when it's absent. If a tool
   isn't in the catalog, it doesn't exist — don't use it.
3. **Write the prompt.** One workflow, guardrails baked in (see below). Test it by pasting
   it into Super Magic against a real tenant.
4. **Open a PR.** Run `python3 tools/gen_catalog.py` first to refresh the catalog. A
   maintainer checks the trigger wording, the tools/connectors, the guardrails, and that
   nothing private slipped in. Merges to `main` auto-sync to the docs site.

## The format (required)

```markdown
---
name: Short Title Case Name
description: When to reach for this skill, in one line — the trigger the agent matches.
category: One of the existing categories
tools: [search_tickets, add_ticket_note]
connectors: []          # e.g. [NinjaOne]; [Zapier: Microsoft Teams]; [] if native-only
---

# Short Title Case Name

**When to use:** One or two concrete situations.

## Prompt

​```
Direct instructions to the agent, with every guardrail inline. Paste-to-run.
​```
```

Guardrails live **inside** the prompt, not in a separate section — a list the agent never
reads won't be followed. Bake in: confidence gates before writes, "show me before you
send/close", "when in doubt, do nothing", result-cap honesty, and "never invent data".

## What gets a skill removed

Be ruthless. A skill is cut, not merged, if it:

- **Can't run** — its core needs a tool not in the catalog, or an unsupported capability
  (SMS, telephony control, RMM script execution). See the catalog's "NOT supported" list.
- **Isn't value-added for an MSP desk** — thin, generic, or something a tech wouldn't
  actually reach for.
- **Duplicates a stronger sibling** — extend the better one and cross-reference; don't ship
  a near-copy.
- **Over-claims Flows** — a cadence/duration/scheduled "unattended" trigger Flows can't do.
  Make it a manual skill or drop it.

## What makes a skill worth merging

- **One workflow.** If describing it needs "and" twice, it's two skills.
- **A real trigger.** `description` is *when to use this*, not a feature summary.
- **Runs in the real world.** Native-only works everywhere; connector-gated works where the
  integration is on (tag it, and degrade cleanly).
- **No private data.** No client/partner names, hostnames, credentials, ticket IDs, or
  environment-specific board/status names. Use placeholders.

The full authoring spec and capability ground truth is in
[`research/AUTHORING.md`](research/AUTHORING.md).
