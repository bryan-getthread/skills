---
name: CW Service Board Conventions
description: For desks synced to ConnectWise Manage running multiple boards — decide which work belongs on which board and execute cross-board moves without losing status, classification, or history.
category: PSA-Specific
tools: [search_tickets, list_boards, list_ticket_statuses, update_ticket, add_ticket_note]
---

# CW Service Board Conventions

Applies to: **ConnectWise Manage**. Multi-board CW desks split work across boards (help desk, escalations/projects, alerts, sales, VIP) and almost everything in CW — statuses, types, workflows, SLAs, team assignments — is board-scoped. Moving a ticket between boards is therefore a small migration, not a field edit.

## When to use

- "Which board should this go on?" on a multi-board CW desk.
- Moving a ticket between boards (help desk → projects, alerts → help desk, service → sales).
- Documenting or sanity-checking the desk's board-routing conventions.

## Steps

1. Re-fetch the ticket with `search_tickets` at full detail and pull the live board list with `list_boards`.
2. Decide the destination from the *nature of the work*, per the desk's conventions: reactive support vs project-sized effort vs monitoring alert vs sales/procurement. If the desk has a routing rules engine, defer to it (`skills/triage-and-routing/board-routing-rules-engine`); this skill covers CW-specific mechanics.
3. Before moving, snapshot what the move will destroy or reset: in CW, a board change re-maps status to the destination board's workflow (often back to its default/new status), and the destination board's types/subtypes/items differ. Record current board, status, and classification in a plain-text `add_ticket_note` — this is the history that survives.
4. Execute the move with `update_ticket`, then in the same pass set a valid destination-board status (`list_ticket_statuses` for the destination) and re-classify per `skills/psa-specific/cw-type-subtype-item`. Never leave a moved ticket sitting on the destination board's default status with stale classification.
5. Check side effects: destination board's SLA and notification workflows now apply; owner/team may need reassignment to someone who works that board.
6. Output: source → destination, the pre-move snapshot note text, the post-move status/classification you set, and anything (owner, SLA) still needing a human decision. For bulk moves, propose the list first and move only after confirmation.

## Guardrails

- **Sync lag:** re-fetch full ticket detail immediately before the move; the ticket may already have been moved or closed in CW.
- A board move without the snapshot note loses context permanently — the note is mandatory, not optional, and must be plain text.
- Never move a ticket to a board whose statuses you have not fetched; you cannot set a safe landing status blind.
- Cross-board moves can restart or void SLA tracking — call it out every time; never move a ticket that is inside an active SLA breach window without explicit confirmation.
- One ticket, one board: if only part of the work belongs elsewhere, split first (`skills/triage-and-routing/multi-issue-ticket-splitter`) rather than bouncing the whole ticket.
- If the desk's board conventions are undocumented, propose the routing and label it a convention *proposal* — do not present it as established policy.

## Cross-references

- Routing logic: `skills/triage-and-routing/board-routing-rules-engine`
- Sales-bound work: `skills/sales-and-quoting/sales-handoff-routing`
