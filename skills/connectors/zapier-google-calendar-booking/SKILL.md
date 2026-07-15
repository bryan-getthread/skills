---
name: Zapier Google Calendar Booking
description: Book remote sessions and onsite visits on a tech's Google Calendar — Find Busy Periods first, then Create Event with a Meet link and attendees, mirrored into the ticket schedule. Use for "book a session with <user>" or "schedule the onsite" when the desk runs Google Calendar, not Outlook.
category: Connectors
tools: [search_tickets, search_contacts, search_members, schedule_ticket, update_schedule_entry, add_ticket_note, "zapier: Google Calendar"]
---

# Zapier Google Calendar Booking

The Google twin of Zapier Outlook Calendar Booking: check real availability with `zapier: Google Calendar "Find Busy Periods"`, book with "Create Detailed Event" (Meet link for remote, site address for onsite), invite attendees so calendars actually update, and mirror the booking into Thread with `schedule_ticket` so dispatch sees it too.

## When to use

- "Book a 30-minute remote session with <user> tomorrow afternoon" — Google Workspace shop.
- "Schedule the onsite at <client>'s office Thursday 9–11."
- A troubleshooting thread lands on "let's get on a call."

## Steps

1. Gather the essentials: attendees (tech via `search_members`, end user via `search_contacts` — addresses from records, not free-typed), duration, remote vs onsite, and the ticket.
2. Check availability honestly: `zapier: Google Calendar "Find Busy Periods"` on the tech's calendar for the proposed window. Busy → offer the nearest free alternatives rather than double-booking. This is the step that makes the booking trustworthy — never skip it.
3. Compose the event: title "<client> — <purpose> (ticket <number>)"; description with the issue summary and what the user should have ready; **remote** → include a Google Meet link on the event; **onsite** → site address in Location, access instructions in the description.
4. Show event details + attendee list for confirmation, then `zapier: Google Calendar "Create Detailed Event"` with attendees so invitations go out.
5. Mirror in Thread: `schedule_ticket` (or `update_schedule_entry` for a reschedule) so the dispatch view matches the calendar; `add_ticket_note` (plain text) with the booked time and meeting-link reference.
6. If the client should pick their own slot instead, offer the self-scheduling routes: `create_timezest_scheduling_request` when the tenant runs TimeZest, or a Calendly one-off link (see Calendly Booking via Zapier).

## Guardrails

- **Member-connected Zapier required** (Google account with rights to the target calendar). Degrade to `schedule_ticket` in Thread plus a drafted invitation the member sends manually. Outlook shop → use Zapier Outlook Calendar Booking instead.
- Find Busy Periods before Create Event, every time; conflicts are surfaced with alternatives, never steamrolled.
- Time zones: state the event time with its zone in the confirmation ("14:00 <site local time>"); the end user's zone comes from their site/contact record. Mismatched zones are the classic booking failure.
- Invitations reach the client — the event description is client-facing text; internal commentary stays on the ticket.
- Reschedules edit the existing event and the Thread schedule entry; never create a second event and orphan the first.
- Task cost: each Zapier call = 2 tasks; one Find Busy Periods + one Create Event per booking is the expected shape.
