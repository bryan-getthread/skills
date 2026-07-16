---
name: Deep Recap
description: Build a comprehensive ticket recap that goes beyond the default template — full message/note/time-entry history, related sibling tickets for the client, a timeline, and current blockers. Answers the commonly requested "give me a REAL recap, not the two-line summary" workflow.
category: Documentation
tools: [search_tickets, list_recap_templates, add_ticket_note, search_clients]
---

# Deep Recap

A recap with the whole picture: everything that happened on the ticket (messages, internal notes, time entries), what else is going on for this client that's related, a dated timeline, and an explicit statement of what's blocking progress right now. Starts from the desk's own recap template so the output still looks like *their* recap — then extends it.

## When to use

- "Recap this ticket properly" / "I'm taking over this ticket, catch me up on everything."
- A long-running or escalated ticket where the default recap is too thin for a handoff or client call.
- Prepping an escalation, a manager review, or a client status meeting from a messy ticket.

## Steps

1. Pull the base format: `list_recap_templates` and use the desk's default recap template's sections as the skeleton, so the deep recap is a superset of what they already expect — not a foreign format.
2. Gather the full history via `search_tickets` on the target ticket: all messages (client and internal), internal notes, status changes, ownership changes, and time entries. If any lookup is capped or truncated, say so in the recap ("history may be incomplete beyond <date>").
3. Find related sibling tickets: `search_tickets` for the same client (and same contact/device where identifiable) with overlapping symptoms or explicit references, recent first. List them as "Related tickets" with number, one-line status, and why they're related — never invent a relationship.
4. Build the recap, extending the template with:
   - **Timeline** — dated bullets of the major events (reported, diagnosed, escalated, waiting periods with durations).
   - **Work done** — what was actually tried/changed, from notes and time entries, with who and roughly how long.
   - **Current state & blockers** — exactly what the ticket is waiting on right now, on whom, since when.
   - **Related tickets** — the sibling list from step 3.
   - **Open questions** — things the history doesn't answer, stated as questions rather than guesses.
5. Post it as an internal note via `add_ticket_note`, formatted per `documentation/note-format-standard` — plain text, PSA-safe, sections as plain headers. In attended mode show it in chat first for review.

## Guardrails

- **Recap only what the record shows.** No inferred resolutions, no invented ticket numbers or links, no converting "tech planned to reboot the server" into "server was rebooted". Gaps go under Open questions.
- Distinguish client-visible messages from internal notes in the timeline — a handoff reader needs to know what the client has and hasn't been told.
- Sibling tickets are context, not conclusions: relate them by evidence (same error, explicit reference, same device), and say the evidence.
- Result-cap honesty: long tickets and busy clients will cap searches; label any section built from capped results as potentially incomplete.
- Plain text throughout — this note is likely to sync to a PSA. No markdown tables, no emojis.
- `create_recap` (the native recap generator) is in-app SuperAgent only; this skill does not depend on it. On any surface, the deliverable is the composed note itself.

## Unattended note

Deep recaps are best attended (the reader usually wants to ask follow-ups). If embedded in a Flow (e.g. auto-recap on escalation), the entire reply is the posted note: skeleton sections always present, "Open questions" instead of guesses, and no questions addressed to a reader who isn't there.
