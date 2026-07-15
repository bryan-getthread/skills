---
name: Meeting Notes to Tickets
description: Pull action items out of Notion meeting notes and turn each into a Thread ticket, with links back to the source meeting.
category: Documentation
tools: [notion-query-meeting-notes, notion-search, notion-fetch, notion-create-comment, create_ticket, search_clients, search_contacts, search_members]
---

# Meeting Notes to Tickets

Close the gap between "we agreed in the meeting" and "it's in the queue": extract action
items from meeting notes and create properly attributed tickets, cross-linked both ways.

## When to use

- "Create tickets from this morning's <client> meeting notes."
- After an internal ops or client-onboarding meeting with a list of committed actions.
- A recurring sync produces action items that keep getting lost.

## Steps

1. Find the meeting: `notion-query-meeting-notes` for the named meeting/date, or `notion-search` + `notion-fetch` if given a page/link. Confirm with the requester it is the right meeting before extracting.
2. Extract only **explicit action items** — commitments with a doable action ("Alex to replace the switch at <client>"). Discussion points, ideas, and "we should someday" remarks are listed separately as "not ticketed" for the requester to promote if wanted.
3. For each action item, resolve the ticket fields:
   - Client: `search_clients` (never assign on name similarity — ambiguous → ask).
   - Contact: `search_contacts` where a client-side person is named.
   - Owner: `search_members` for the person committed in the notes; unassigned if unclear.
   - Title/description per the desk's intake standard (see ticket-intake-formatting), with the description quoting the action item and linking the source meeting page.
4. Present the full proposed ticket list — title, client, owner, due context — and get one explicit confirmation before creating anything.
5. Create the tickets with `create_ticket`, then close the loop: add a comment on the meeting page via `notion-create-comment` listing each action item with its new ticket number.
6. Output the summary: tickets created (with numbers), items skipped and why, and any unresolved owners/clients needing human assignment.

## Guardrails

- Confirm before creating: never bulk-create tickets from notes without the requester approving the list. One confirmation for the batch is fine; silent creation is not.
- Zero-assumption on commitments: only ticket items the notes explicitly assign or agree to. Do not infer owners, due dates, or scope.
- Never assign a client or contact on name similarity alone; low confidence → leave blank and flag.
- Notion tools are member-authenticated and `notion-query-meeting-notes` requires a Notion plan with Notion AI — if unavailable, ask the requester to paste the notes and proceed from the paste (skip the backlink comment).
- Do not duplicate: check for an existing ticket when a note references one ("continue ticket about the printer") before creating a sibling.
