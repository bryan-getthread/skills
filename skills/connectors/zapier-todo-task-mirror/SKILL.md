---
name: Zapier To Do Task Mirror
description: Mirror ticket follow-ups into a tech's Microsoft To Do list and complete them when the ticket resolves — the personal-task bridge for techs who live in the Microsoft task ecosystem. Use for "put my ticket follow-ups in To Do", "add a task to call <user> Friday", or clearing mirrored tasks on resolution. (Planner has no Zapier integration — To Do is the Microsoft option.)
category: Connectors
tools: [search_tickets, search_members, add_ticket_note, "zapier: Microsoft To Do"]
---

# Zapier To Do Task Mirror

Tickets hold the desk's truth; a tech's day runs on their task list. This skill mirrors ticket follow-ups into Microsoft To Do — `zapier: Microsoft To Do "Create Task"` with the ticket reference baked in — and closes the loop by completing the mirrored task when the ticket resolves, so the list never fills with ghosts of finished work.

## When to use

- "Mirror my open ticket follow-ups into my To Do list."
- "Add a task: call <user> Friday about ticket <number>."
- On resolution passes: "clear the To Do tasks for tickets that closed this week."
- Anyone asking for **Planner**: no Zapier integration for Planner exists — offer To Do (personal), or Linear/Jira for team-level tracking, and say why.

## Steps

1. Confirm whose list and which list: the tasks land in the *connected member's* To Do (the connection is personal — one member's Zapier cannot write to another tech's list; mirroring for someone else means they run it themselves). Default to the tenant's convention (e.g. a "Ticket follow-ups" list) or ask once.
2. Gather the follow-ups: from `search_tickets` (the member's open tickets with pending next actions) or the single item the member states.
3. **Mirror format — the dedupe contract:** task title `[<ticket-number>] <action>` — the bracketed ticket number is the join key for everything that follows. Due date only when the ticket/member states one; a fabricated due date is a fabricated commitment. Notes field carries the one-line context.
4. **Dedupe before creating:** `zapier: Microsoft To Do "Find Task"` for the ticket number. Exists and open → skip, report. Never double-mirror.
5. Show the batch (task, due date, list) for confirmation, then create per task. `add_ticket_note` (plain text) on ticket-driven items: "follow-up mirrored to To Do".
6. **Completion sweep:** given resolved/closed tickets, Find Task by ticket number and "Complete Task" for each match — after confirming the ticket state from Thread, not from memory. Report completed and not-found counts.
7. The mirror is one-way by design: Thread is the system of record; To Do is a reflection. A task ticked in To Do does not resolve the ticket — resolution flows ticket → task, never the reverse.

## Guardrails

- **Member-connected Zapier required** (the member's own Microsoft account). Degrade: output the follow-up list as formatted text for manual entry into any task app.
- The `[ticket-number]` convention is non-negotiable — without it the completion sweep is guesswork and the list rots.
- One-way mirror: never resolve, update, or annotate tickets *because* a To Do task changed; never mark a To Do task complete while its ticket is still open.
- Task titles may sync to the member's visible devices — client-safe phrasing, no credentials, no sensitive end-user detail.
- Planner honesty: state plainly that Planner has no Zapier integration; don't emulate it badly — recommend To Do, Linear, or Jira per the need.
- Task cost: each Zapier call = 2 tasks, and mirroring is per-task (find + create ≈ 4 Zapier tasks per follow-up) — mirror the follow-ups that need the list, not every open ticket.
