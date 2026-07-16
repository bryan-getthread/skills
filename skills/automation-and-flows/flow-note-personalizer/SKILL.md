---
name: Flow Note Personalizer
description: Replace a Flow's static "add note" text with an AI step that resolves the real owner and ticket context and composes a note with actual names and specifics. Answers the commonly requested "make our flow notes say something useful instead of the same canned line" partner workflow.
category: Automation & Flows
tools: [search_tickets, search_members, add_ticket_note]
---

# Flow Note Personalizer

A Flow's built-in Note action writes the same fixed text every time. This skill is what a flow calls instead: it reads the ticket, resolves who owns it, and writes a note that names the real people and the real situation — so the internal record is worth reading.

## When to use

- A flow currently posts a generic note ("Ticket routed by automation") and the desk wants it to actually describe what happened.
- "Make the flow note mention who it was assigned to and why."
- Standardizing internal notes across flows so they carry owner and context, not boilerplate.

## Steps

1. Read the ticket with `search_tickets`: current owner/assignee, board, status, client, and the triggering change (what the flow just did).
2. Resolve the people involved to real names with `search_members` — the assigned resource, and the dispatcher/owner if different. Never leave a "@owner" placeholder in the final note.
3. Compose a compact plain-text note that states: what the flow did, who now owns the ticket, and the one relevant piece of context (priority set, board moved, agreement matched). Keep it factual and internal — no client-facing tone.
4. Post it via `add_ticket_note`.

## Guardrails

- Names come from `search_members` only. If a person can't be resolved, write the role plainly ("unassigned") rather than inventing a name or leaving a template token.
- The note describes only what actually happened on this ticket — do not assert actions the flow did not take.
- One note per flow run; this skill replaces the flow's static Note action, it does not add a second note on top of it.
- Plain text only — no markdown or emojis (PSA compatibility).
- This skill writes a note and nothing else — no status/owner/priority changes.

## Unattended (Flows) variant

- Fires on whatever ticket event the parent flow is triggered by (create, status change, priority change, assignment, reply, note) — it inherits the flow's trigger, it does not add one.
- Your entire reply is the note, posted verbatim — plain text, no narration, no markdown.
- If context is too thin to say anything more useful than the boilerplate it replaces, post a minimal factual note (what changed + owner) rather than padding with speculation.
- Resolve names before writing; never emit a template placeholder in the posted note.
