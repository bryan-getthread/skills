<!--
Canonical Skill template. Copy this into skills/<category>/<slug>/SKILL.md and fill it in.
Every skill in this library follows this exact shape. See CONTRIBUTING.md for the rules
and research/tool-catalog.md for the list of tools/connectors you may reference.
Delete this comment block in your copy.
-->
---
name: Short Title Case Name
description: When to reach for this skill, in one line — this is the trigger the agent matches.
category: One of the categories in README.md
tools: [search_tickets, add_ticket_note]
connectors: []            # e.g. [NinjaOne] or [Zapier: Microsoft Teams]; [] if native-only
---

# Short Title Case Name

**When to use:** One or two concrete situations, in the words a tech would actually use.

## Prompt

```
The runnable prompt — paste this whole block into Super Magic. Write it as direct
instructions to the agent: what to read, what to produce, and every guardrail baked in
inline (confidence gates, "show me before writing", "when in doubt do nothing",
result-cap honesty, never invent data). Reference tools by name where a call happens.
Keep it tight — if it needs the word "and" twice to describe, it's probably two skills.
```
