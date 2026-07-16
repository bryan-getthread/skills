---
name: Project Conversion Inbox Cleanup
description: When a ticket is converted to a project type or moved to a project board, set the documented attribute that removes it from live inbox/queue views so day-to-day dispatch isn't cluttered by long-running project work. Answers the commonly requested "keep project tickets out of the reactive inbox" partner workflow.
category: Automation & Flows
tools: [search_tickets, update_ticket, add_ticket_note]
---

# Project Conversion Inbox Cleanup

A ticket that becomes project work shouldn't keep surfacing in the reactive queue. This skill fires when a ticket is converted to a project type/board and applies the desk's documented inbox-exclusion attribute, so project tickets live on the project board without polluting live dispatch views.

## When to use

- Tickets converted to project work still appear in the front-line inbox/dispatch views.
- "When something becomes a project, get it out of our reactive queue."
- A flow should tidy the inbox automatically on conversion to a project board or type.

## Steps

1. Read the ticket with `search_tickets` and confirm the conversion actually happened: it is now a project type or sits on a project board (see `skills/psa-specific/cw-project-tickets` for the ConnectWise idiom). If it is not genuinely project work, stop.
2. Identify the desk's **documented inbox-exclusion attribute** — the specific field/value this desk uses to keep a ticket off live inbox/queue views (e.g. a queue/board reassignment, a view-scoping status, or a flag the desk's views filter on). This is configured alongside the skill; do not invent a field.
3. Apply that attribute with `update_ticket`.
4. Post a plain-text audit note via `add_ticket_note`: that the ticket was recognized as project work and which inbox-exclusion attribute was set.

## Guardrails

- **Do not invent the mechanism.** Thread desks scope inbox/queue views differently; apply only the documented attribute this desk uses. If it isn't documented, stop and note that no exclusion attribute is configured rather than guessing at a field.
- Confirm the conversion before acting — never pull an ordinary reactive ticket out of the inbox because it merely mentions a project.
- The attribute change and audit note are the only writes. Do not change status to something that would hide the ticket from the project team too, and do not close it.
- Notes plain text (PSA compatibility).

## Unattended (Flows) variant

- Fires on the **board-change or type-change event** (ticket moved to a project board / set to project type) — supported ticket events, not a timer.
- Your entire reply is the audit note, verbatim: `PROJECT INBOX CLEANUP. Set <attribute>. Removed from live views.` or `NO ACTION. Reason: <not project work | no exclusion attribute configured>.`
- Deterministic stops: not genuinely project work → no action; no documented exclusion attribute → no action.
- One application per conversion event.
