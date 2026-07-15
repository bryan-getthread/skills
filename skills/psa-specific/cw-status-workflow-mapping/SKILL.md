---
name: CW Status Workflow Mapping
description: For desks synced to ConnectWise Manage — map Thread statuses to CW board statuses, make only safe status transitions, and reconcile closed-in-CW-but-open-in-Thread drift.
category: PSA-Specific
tools: [search_tickets, list_boards, list_ticket_statuses, update_ticket, add_ticket_note]
---

# CW Status Workflow Mapping

Applies to: **ConnectWise Manage**. Thread statuses mirror the CW board's status list, and each CW board has its own workflow (which statuses exist, which are closed-family, which fire notifications or stop SLA clocks). This skill keeps status changes inside that workflow and reconciles drift between the two systems.

## When to use

- "Move this ticket to <status>" on a CW-synced desk and you need the safe equivalent.
- A ticket shows different statuses in Thread and in ConnectWise.
- Building or sanity-checking the desk's Thread↔CW status map for a board.
- Before any bulk status change on a CW-synced board.

## Steps

1. Re-fetch the ticket with `search_tickets` and read its full detail — never act on a status seen earlier in the conversation or in a list view. CW→Thread sync can lag by minutes.
2. Pull the live status list for the ticket's board with `list_ticket_statuses` (and `list_boards` if the board is ambiguous). CW statuses are **per-board**: a status that exists on the service board may not exist on the projects board.
3. Map the requested transition onto a status that actually exists on that board. Prefer the desk's documented map; if none exists, propose the closest match and say it is a proposal.
4. Classify the target status before moving: is it closed-family (closes the ticket in CW), does it stop the SLA clock (waiting-type statuses), does it fire client notifications (CW workflow rules trigger on status)? Call out each side effect in your proposal.
5. If Thread and CW disagree — most commonly closed in CW, still open in Thread — treat **the PSA as master**. Do not reopen in CW to match Thread. Instead: re-fetch once more to rule out lag; if the divergence is real, align Thread to the CW state with `update_ticket` and record what you did with `add_ticket_note` (plain text).
6. Output: current status in each system, the proposed transition, its side effects, and the exact `update_ticket` change — then apply only after confirmation.

## Guardrails

- **Sync lag:** always re-fetch full ticket detail with `search_tickets` immediately before trusting or changing status. A stale list result is not evidence of divergence.
- **PSA is always master.** When the systems disagree, Thread moves to match CW, never the reverse — unless the desk has explicitly documented Thread as master for that board.
- Never set a status name that `list_ticket_statuses` did not return for that board. Do not invent statuses.
- Never move a ticket into a closed-family status as a side effect of "just updating status" — closing is a deliberate act with its own QA gate (see `skills/qa-and-closure/closure-note-completeness`).
- Notes that sync to CW must be plain text — no markdown, no emojis.
- If the desk uses CW workflow rules keyed on status, warn that a status change may auto-email the client; when unsure, propose and stop.

## Cross-references

- Generic reconciliation pattern: `skills/psa-specific/psa-is-master-reconciliation`
- Full divergence sweep: `skills/psa-specific/cw-sync-lag-audit`
