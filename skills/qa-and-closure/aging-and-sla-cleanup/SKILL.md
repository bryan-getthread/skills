---
name: Aging, SLA & Follow-Up
description: Sweep open tickets for real staleness — measured by last meaningful client-facing update, not last touch — separate MSP-owned stalls from legitimate waits, nudge the assigned tech, and drive clean follow-up or closure with sign-off.
category: QA & Closure
tools: [search_tickets, add_ticket_note, update_ticket, schedule_ticket, search_members, list_boards, list_ticket_statuses]
---

# Aging, SLA & Follow-Up

The recurring hygiene sweep for tickets going quiet. Distinguishes tickets that are genuinely stalled on the MSP's side from tickets correctly waiting on a client, vendor, or scheduled date — and produces a prioritized action list instead of a raw age report.

## When to use

- Run on a cadence (daily or weekly) to catch tickets going quiet or drifting toward SLA trouble.
- "What's aging in the queue?" / "clean up the stale tickets" / "which tickets have we dropped?"
- Before a service review, to clear the backlog of forgotten tickets honestly.

## Steps

1. Enumerate open tickets with `search_tickets`, splitting searches per board and per status (use `list_boards` / `list_ticket_statuses` to enumerate) so result caps do not silently truncate the sweep. If any search may have hit a cap, say so in the output.
2. For each ticket, compute the **last meaningful client-facing update**: the most recent client-visible message or substantive progress note. Internal notes, time entries, status flips, and automated touches do NOT reset the clock — a ticket "touched" daily by a bot can still be stale.
3. Classify each stale ticket's ownership:
   - **MSP-owned stall** — the ball is in our court: client replied and we went quiet, work promised but not documented, unassigned and aging.
   - **Legitimate wait** — client response pending WITH a follow-up actually sent, vendor RMA or third-party dependency documented, or a future scheduled service date in a proper scheduled status. Verify the wait is documented, not assumed.
4. For each MSP-owned stall, `add_ticket_note` (internal, plain text) @mentioning the assigned technician — resolve them with `search_members` — stating the days since the last meaningful client-facing update and the single next expected action. If unassigned, flag for dispatch instead.
5. Pull tickets nearing or past an SLA target out of the routine list and flag them for immediate attention at the top of the output — imminent breaches are not routine aging.
6. For legitimate waits with no future touchpoint, offer to `schedule_ticket` a follow-up date rather than leaving them adrift.
7. Output a prioritized list: SLA-imminent first, then MSP-owned stalls (oldest meaningful-update first), then legitimate waits with their next touchpoint. One recommended action per ticket: nudge, escalate, schedule, or route to closure.
8. Present everything for sign-off before acting. Never bulk-close, bulk-reassign, or bulk-send follow-ups without explicit confirmation.

## Guardrails

- Last touch is not last update: never mark a ticket healthy because an internal note or automation touched it recently.
- Never bulk-close without sign-off — closure of no-response tickets belongs to the No-Response Closure Sequence skill after documented attempts.
- Skip tickets parked on a future scheduled service call in a proper status; do not nag a correctly scheduled ticket.
- @mention notes are factual nudges, not blame: state the age and the next action, nothing about the tech's performance.
- Disclose result caps rather than presenting a truncated sweep as complete.
- If write tools are disabled for this tenant, deliver the action list in chat only.

## Consolidates

Stalled-ticket auditors, aging-ticket workflows, close-stale-tickets sweeps, SLA-risk surfacing, and follow-up-on-waiting variants. Cadenced client follow-ups, no-response closure, waiting-status audits, and SLA tiering have dedicated skills in this category; this is the umbrella sweep that finds the work.
