---
name: Auto Priority Classification
description: Set a ticket's priority from its title and description against the partner's own priority definitions — built for alert boards and intake flows.
category: Triage & Routing
tools: [search_tickets, list_ticket_priorities, update_ticket, add_ticket_note]
---

# Auto Priority Classification

Read a ticket, match it against the desk's written priority definitions, and set exactly one priority — or leave it untouched when the definitions do not clearly apply.

## When to use

- An alert board needs every incoming ticket stamped with a priority automatically.
- "What priority should this be?" against the desk's own P1/P2/P3 definitions.
- A flow fires on ticket creation and needs one deterministic priority write.

## Steps

1. Read the ticket's title and description with `search_tickets`. If the description is empty, read the earliest message.
2. Load the tenant's priority names with `list_ticket_priorities`; map them to the desk's priority definitions (as configured in this skill's install or provided in the prompt).
3. Match the ticket against each definition in severity order, highest first. Use the described impact and scope (how many users, business-critical system, security implication), not tone or keywords alone.
4. Forcing rules override the match: confirmed security issues and multi-user/site-wide outages classify at the highest applicable priority.
5. If exactly one definition fits, set it with `update_ticket` and post a one-line plain-text note with `add_ticket_note` naming the definition that matched.
6. If none fits cleanly or two fit equally, do not change the priority; note that classification was inconclusive.

## Guardrails

- Choose only from the priorities returned by `list_ticket_priorities` — never invent a level.
- Priority is the only field this skill writes. No board, status, owner, or type changes.
- Never downgrade an existing priority that a human set higher than your match.
- Vague or empty tickets → no change, one note. Do not guess.
- Notes are plain text for PSA compatibility.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the internal note — no narration, no questions. Format: `PRIORITY: <name>. Matched definition: <one line>.` or `PRIORITY: unchanged. Reason: <one line>.`
- Exactly one `update_ticket` call per run, or none. When in doubt, do nothing.
- If the ticket already has a priority note from this skill in the current thread, stop without writing.
