---
name: Zapier ClickUp Tasks
description: For clients running on ClickUp — mirror project-worthy tickets as tasks in the client's agreed Space/List, sync status through their statuses and comments, and close the completion loop both ways. Use for "add this to their ClickUp", "what's the ClickUp task status", or clients coordinating project work in ClickUp.
category: Connectors
tools: [search_tickets, add_ticket_note, update_ticket, "zapier: ClickUp"]
---

# Zapier ClickUp Tasks

ClickUp shops organize work as Workspace → Space → Folder → List → task, with per-List custom statuses. This skill mirrors project-worthy tickets as tasks in the client's agreed List, writes status using the statuses *that List actually has*, and keeps a two-way trail — the same mirror discipline as the Asana/Monday variants, adapted to ClickUp's hierarchy.

## When to use

- "<client> manages the project in ClickUp — mirror ticket <number> as a task."
- A ticket is one work item in a client-led initiative tracked in ClickUp.
- "Check their ClickUp task and update our ticket."
- Completion on either side needs reflecting on the other.

## Steps

1. **Verify before promising (ClickUp's Zapier inventory is not pre-verified in this library):** run the Zapier Action Discovery pattern first — confirm actions equivalent to "Create Task", "Add Comment", "Update Task" (or a status-change action), and a "Find Task" lookup exist and are enabled before describing the workflow. Note what's actually available; if status changes aren't writable, the sync degrades to comments and the member should know before agreeing with the client.
2. Confirm the target: the exact List (ClickUp statuses are List-scoped) and the client's status names for in-progress/blocked/done. Their hierarchy and status labels come from the mirroring agreement or from asking — never from browsing their Workspace and guessing.
3. **Qualify:** mirror project-worthy work tied to the client's initiative, not routine support tickets. Unsure → ask the member.
4. **Find before create:** search the List for an existing task carrying this ticket's reference. Found → comment/update it rather than creating a duplicate.
5. Create the task: client-safe name in their convention, description with the work summary, current state, and the Thread ticket reference. Show for confirmation, then create. Capture the returned task ID and record it on the ticket via `add_ticket_note` (plain text) — the permanent handle for every later sync.
6. Status sync outbound: on material ticket progress, set the task's status to the matching name from *their* List's status set and/or add a comment — client-safe, dated, ticket-referenced. Use only status names confirmed to exist on that List.
7. Status sync inbound: on request or per pass, read the task and relay material changes (status moved, assignee, due date, blocking comments from their team) onto the ticket via `add_ticket_note` with "per ClickUp task <ID>" provenance.
8. **Completion loop:** ticket resolves → comment the outcome and set the client's done-status only if the agreement says the MSP closes its tasks; otherwise comment "done from our side". Client closes the task first → note it on the ticket and confirm with the requester before resolving.

## Guardrails

- **Member-connected Zapier required** (the client's ClickUp Workspace, standing client-granted access). Degrade: output the composed task name, description, status, and comment text for someone with ClickUp access, and note on the ticket that the mirror is pending.
- Their Workspace: touch only the agreed List and tasks this skill created or was pointed at. Never restructure Spaces/Folders/Lists, reassign their people, or edit their other tasks.
- Status names are List-scoped and belong to the client — writing a status that doesn't exist on their List fails or corrupts their workflow; confirm the set once and reuse it.
- Everything written is client-visible: professional wording, no internal blame, no credentials, no other clients' data.
- Task-ID-on-ticket is non-negotiable.
- Task cost: each Zapier call = 2 Zapier tasks — find + create per mirror, one call per sync touch; combine status + comment into one update where the action allows, and never poll.
