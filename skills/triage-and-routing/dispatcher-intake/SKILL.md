---
name: Dispatcher Intake
description: Attended chat intake for dispatchers — describe the issue conversationally and get back a properly created ticket: KB/docs/history checked, board/type/priority set, summary written, first-touch note attached. Answers the commonly requested "let me open tickets by just telling the agent what happened" workflow.
category: Triage & Routing
tools: [search_knowledge_base, search_itglue, search_hudu, search_tickets, search_clients, search_contacts, list_boards, list_ticket_statuses, list_ticket_priorities, create_ticket, add_ticket_note]
---

# Dispatcher Intake

Conversational ticket creation for the person answering the phone: the dispatcher describes the issue in their own words, the agent asks the one or two questions that matter, checks documentation and history while they talk, and creates a well-formed ticket with a research-backed first note. Complement of `triage-and-routing/new-ticket-first-touch`, which works ON a ticket that already exists — this skill is the step before: no ticket yet, a human relaying an issue live. For turning messy written notes into structure (rather than a live conversation), see `documentation/handwritten-notes-structurer`.

## When to use

- A dispatcher on a call: "Someone from <client> just called, their label printer in the warehouse won't print, started this morning."
- Walk-up / hallway intake: a tech relays an issue verbally and nobody wants to fill out a form.
- Phone-heavy desks that want every call to land as a consistent, classified ticket.

## Steps

1. Take the description as given, then ask only for what's missing and load-bearing: which client, who reported it (contact), what's affected, since when, how urgent it feels. One compact round of questions — dispatchers are usually mid-call.
2. Resolve entities as you go: `search_clients` for the client, `search_contacts` for the reporter (offer the closest matches rather than guessing between similar names).
3. Research in parallel with the conversation:
   - `search_tickets`: similar past tickets for this client (same symptom/device) and any OPEN ticket that might make this a duplicate — if a likely duplicate exists, say so and let the dispatcher decide before creating anything.
   - `search_knowledge_base`, and `search_itglue` / `search_hudu` where enabled: known fixes, client-specific runbooks, environment notes.
4. Propose the ticket before creating it: board (`list_boards`), type/subtype per the intake-classification-tree logic, priority (`list_ticket_priorities`) with a one-line justification, a summary line in the desk's format, and a clean description from the dispatcher's words. Dispatcher confirms or corrects.
5. `create_ticket` with the confirmed fields, then `add_ticket_note` with the first-touch research note (plain text): similar past tickets found (numbers + one-line outcomes), relevant KB/docs references, and suggested first steps for the assigned tech.
6. Hand back: ticket number, where it landed, and anything the dispatcher should tell the caller (expected next step — no time promises unless the desk's SLA config states one).

## Guardrails

- Attended by design: the dispatcher confirms client, contact, and priority before `create_ticket` fires. Never create on an unresolved client or contact match.
- Duplicate check is mandatory: creating a second ticket for an issue already open wastes a tech's afternoon. Surface candidates; the dispatcher decides.
- The research note cites only what searches actually returned — no invented KB articles, ticket numbers, or doc links. Empty research → say "no similar tickets or docs found", which is itself useful.
- Result-cap honesty: history checks on busy clients may cap; label the similar-tickets list as partial when it does.
- Notes are plain text (PSA-safe). The ticket description preserves the dispatcher's factual content — clean it up, don't editorialize it.
- Degradation: `search_itglue` / `search_hudu` appear only when those integrations are enabled; skip silently and rely on KB + ticket history when absent.
