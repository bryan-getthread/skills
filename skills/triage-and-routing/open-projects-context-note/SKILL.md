---
name: Open Projects Context Note
description: On ticket create, search for the client's open project-board tickets and, if any exist, post an internal context note so the tech knows this reactive ticket may relate to active project work. Answers the commonly requested "warn us when a new ticket touches an in-flight project" partner workflow.
category: Triage & Routing
tools: [search_tickets, search_clients, add_ticket_note]
---

# Open Projects Context Note

A reactive ticket for a client with an active project is often connected to it — a migration side effect, a rollout question. This skill checks, on create, whether the client has open project-board tickets and, if so, posts an internal note pointing to them, so the tech has that context before they start.

## When to use

- Clients with active projects file reactive tickets that turn out to be project-related.
- "Tell me if this client has an open project when a new ticket comes in."
- Preventing project-related work from being handled in isolation on the service desk.

## Steps

1. Read the new ticket with `search_clients` / `search_tickets` to identify the client (and site if relevant).
2. Search for the client's **open tickets on the desk's project board(s)** via `search_tickets`, scoped to project boards and non-closed statuses (see `skills/psa-specific/cw-project-tickets`). Split per board and disclose result caps.
3. If none exist, do nothing.
4. If one or more exist, post a plain-text internal note via `add_ticket_note` listing the open project tickets (number + short title) and a one-line flag: "this client has active project work — check whether this ticket relates before proceeding." Do not assert that it *is* related; that's the tech's call.

## Guardrails

- **Context only, no routing.** This skill posts an informational note; it does not move the ticket to the project board, link tickets, or change status/owner. The tech decides.
- List only real open project tickets found via `search_tickets` — never infer or fabricate a project that isn't in the results.
- If the search hits a result cap, say so in the note rather than implying the list is complete.
- Internal note only — never client-facing.
- One context note per ticket; if one already exists, do not add another.
- Notes plain text (PSA compatibility).

## Unattended (Flows) variant

- Fires on **ticket create** — a supported event.
- Your entire reply is the note, verbatim: `RELATED PROJECT WORK: <#n title>; <#n title>. Check whether this ticket relates before proceeding.` or output nothing if the client has no open project tickets.
- Deterministic stops: no open project tickets → nothing; note already present → nothing.
- Never claim relatedness — only surface the open project tickets.
