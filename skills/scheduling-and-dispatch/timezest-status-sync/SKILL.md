---
name: TimeZest Status Sync
description: When a "TimeZest scheduled" note lands on a ticket, verify the booking actually exists via the TimeZest API and move the ticket to the Scheduled status — so the queue reflects real bookings, not just a note. Answers the commonly requested "flip tickets to Scheduled when TimeZest books them" partner workflow.
category: Scheduling & Dispatch
tools: [search_tickets, get_timezest_scheduling_requests, update_ticket, list_ticket_statuses, add_ticket_note]
---

# TimeZest Status Sync

A TimeZest booking drops a note on the ticket, but the ticket often stays in its old status — so the queue doesn't show what's actually scheduled. This skill verifies the booking against TimeZest and sets the ticket to the desk's Scheduled status, keeping status honest with reality.

## When to use

- Clients book via TimeZest but tickets don't move to a Scheduled status automatically.
- "When TimeZest books a slot, mark the ticket Scheduled."
- Keeping the dispatch queue accurate about what's actually on the calendar.

## Steps

1. On the "TimeZest scheduled" note, read the ticket with `search_tickets`.
2. **Verify the booking is real** with `get_timezest_scheduling_requests` for the ticket — confirm a scheduled (not merely requested/pending) appointment exists. If nothing confirmed comes back, do nothing — a note alone is not proof.
3. Confirm the desk's Scheduled status exists via `list_ticket_statuses` (the documented "Scheduled" status for this board).
4. If the booking is verified and the ticket isn't already Scheduled, set it with `update_ticket`.
5. Post a plain-text note via `add_ticket_note`: booking verified via TimeZest, status moved to Scheduled, with the appointment time if available.

## Guardrails

- **Verify before writing.** Trust the TimeZest API (`get_timezest_scheduling_requests`), not the note text — a note can be stale or a leftover from a cancelled request. No confirmed booking → no status change.
- Status change + one note are the only writes — never create/reschedule the booking here (that's `skills/scheduling-and-dispatch/timezest-booking`).
- If the ticket is already Scheduled, do nothing.
- **Degradation:** the TimeZest tools appear only when the integration is enabled for the tenant. If absent, do nothing and note that TimeZest verification isn't available so a human can set status manually.
- Notes plain text (PSA compatibility).

## Cross-references

- Creating the booking: `skills/scheduling-and-dispatch/timezest-booking`

## Unattended (Flows) variant

- Fires on the **note-added event** (the "TimeZest scheduled" note) — a supported ticket event.
- Your entire reply is the note, verbatim: `SCHEDULED. TimeZest booking verified for <time>. Status set to Scheduled.` or `NO ACTION. Reason: <no confirmed booking | already scheduled | status missing | TimeZest unavailable>.`
- Deterministic stops: booking not confirmed via API → no action; already Scheduled → no action; Scheduled status missing → no action; TimeZest integration absent → no action.
