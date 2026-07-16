---
name: Zapier Asana Client Projects
description: For clients whose internal work lives in Asana — mirror project-worthy tickets as Asana tasks in the client's agreed project, relay status both ways as notes, and close the loop when either side finishes. Use for "put this on their Asana board", "what does their Asana task say now", or clients who coordinate migrations/projects in Asana.
category: Connectors
tools: [search_tickets, add_ticket_note, update_ticket, "zapier: Asana"]
---

# Zapier Asana Client Projects

Some clients run their internal world in Asana, and a ticket that represents real project work (a migration step, an office move task, a rollout item) is invisible to them until it exists as a task on their board. This skill mirrors project-worthy tickets into the client's agreed Asana project, keeps a two-way status trail, and closes the loop — without becoming a firehose that mirrors every password reset.

## When to use

- "<client> tracks the office move in Asana — add our cabling ticket to their project."
- A ticket is one work item inside a client-led project that lives in Asana.
- "Check what their Asana task says and update our ticket."
- Completion on either side needs to be reflected on the other.

## Steps

1. **Verify before promising (Asana's Zapier inventory is not pre-verified in this library):** run the Zapier Action Discovery pattern first — confirm actions equivalent to "Create Task", "Update Task", "Add Comment/Story to Task", and a "Find Task"/"Find Project" lookup actually exist and are enabled (or pinned, on a curated server) before describing any workflow to the member. If a needed action is missing, say so plainly and stop at the degrade path.
2. Confirm the target: the client's agreed project (and section, if their board uses them). The project is part of the cross-mirroring agreement with the client — never pick a project by name-guessing their workspace.
3. **Qualify the ticket:** mirror only project-worthy work — items that belong to a client-led initiative, not routine support tickets. When in doubt whether a ticket qualifies, ask the member; a client board cluttered with MSP noise erodes the arrangement.
4. **Find before create:** search the project for an existing task carrying this ticket's reference. Found → comment on it rather than creating a twin.
5. Create the task: client-safe title in their naming convention, description with what the desk is doing, the current state, and the Thread ticket reference. Show the composed task for confirmation, then create. Capture the returned task ID/URL and record it on the ticket via `add_ticket_note` (plain text) — that ID is the permanent handle for all future syncs.
6. Status sync outbound: on material ticket progress (started, blocked, waiting on client, resolved), add a comment to the Asana task — client-safe, dated, ticket-referenced. Material changes only; not every internal note.
7. Status sync inbound: on request or per pass, read the task and relay material changes (completed, reassigned, due date moved, blocking comments) onto the ticket via `add_ticket_note` with "per Asana task <ID>" provenance.
8. **Completion loop:** when the Thread ticket resolves, comment the outcome on the Asana task and mark it complete only if the mirroring agreement says the MSP closes its own tasks — otherwise comment "done from our side" and leave completion to the client. When the client completes the task first, note it on the ticket and confirm with the requester before resolving.

## Guardrails

- **Member-connected Zapier required** (connected to the client's Asana workspace under standing, client-granted access). Degrade: output the composed task/comment text for someone with Asana access to paste, and note on the ticket that the mirror is pending.
- It is the client's board: touch only the agreed project, and only tasks this skill created or was pointed at. Never rearrange their sections, assignees, or other tasks.
- Everything written to Asana is client-visible: plain professional wording, no internal blame, no credentials, no other clients' information.
- The task-ID-on-ticket rule is non-negotiable; without it every later sync is a fragile text search in someone else's workspace.
- Do not invent project names, section names, or assignees — the client's taxonomy comes from the agreement or from asking.
- Task cost: each Zapier call = 2 Zapier tasks — expect one find + one create per mirror and one call per sync touch; sync on material change, never on a polling loop.
