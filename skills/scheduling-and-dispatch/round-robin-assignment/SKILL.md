---
name: Round-Robin Assignment
description: Distribute incoming tickets fairly across a named technician roster in rotation, honoring exclusion rules — runnable unattended inside a Flow.
category: Scheduling & Dispatch
tools: [search_tickets, search_members, update_ticket, add_ticket_note]
---

# Round-Robin Assignment

Fair rotation: each new ticket goes to the next technician in the roster, so distribution is even by design rather than by whoever happens to look free.

## When to use

- The desk assigns a board's intake by rotation ("round-robin the helpdesk board across <roster>").
- A Flow fires on ticket creation for a board that uses rotation instead of load scoring.
- A dispatcher wants to spread a batch of tickets evenly across a named group.

## Steps

1. Establish the roster. Use the explicit roster the desk configured for this skill (a named list of members); verify each name resolves via `search_members`. If no roster is configured, ask for one — do not invent a roster from "everyone on the board".
2. Determine the rotation position: find the most recent ticket on this board assigned by this rotation (via `search_tickets`, newest assigned first, or the rotation marker in its internal note) and pick the next roster member after that assignee, wrapping at the end.
3. Apply exclusions before assigning, and skip to the next in rotation if the candidate is: the ticket's requester, inactive, marked out/on PTO, or excluded by a client-specific routing rule or pinned exclusion for this ticket's client. A skipped tech keeps their place — they get the next eligible ticket.
4. If every roster member is excluded, stop: report (or note, in unattended mode) that no eligible candidate exists and leave the ticket unassigned.
5. Assign via `update_ticket` and record a plain-text internal note via `add_ticket_note` marking the rotation, e.g. "Round-robin: assigned <tech> (position 3 of 5; skipped <tech>, PTO)." — this note is also the rotation state for the next run.
6. For batch mode, continue the rotation across the batch (ticket 1 → next tech, ticket 2 → the one after) and output the full distribution table for confirmation before applying.

## Guardrails

- The roster is explicit. Never substitute or add members the desk didn't name.
- Fairness is auditable: every assignment note states position and any skips, and skipped techs stay next in line.
- Never assign to the requester, inactive members, or anyone excluded by a routing rule or pinned exclusion.
- Client-specific routing rules beat the rotation — if a rule pins this client's work elsewhere, follow the rule, note it, and do not advance the rotation.
- Never reassign a ticket that already has an owner.
- If the last-rotation lookup is ambiguous (no marker found, or search hit a result cap), in attended mode ask; in unattended mode start from the top of the roster and say so in the note.

## Unattended (Flows) variant

- Your entire reply is posted as the internal note — plain text, no narration, no questions.
- Deterministic behavior only: next eligible member in the configured roster order, skips noted. No judgment calls about "fit".
- Already assigned, empty roster, or all members excluded → do nothing except (in the all-excluded case) a single note: "Round-robin: no eligible roster member (reasons). Left unassigned."
- Touch only the assignee and the note — never status, priority, or the client-facing thread.
