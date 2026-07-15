---
name: Zapier Outlook Calendar Booking
description: Book remote sessions and onsite visits on the tech's Outlook calendar — Create Event with a Teams meeting link, attendees added, ticket referenced. Use for "book a remote session with <user>", "schedule the onsite for Tuesday", or "put the follow-up call on my calendar".
category: Connectors
tools: [search_tickets, search_contacts, search_members, schedule_ticket, add_ticket_note, "zapier: Microsoft Outlook"]
---

# Zapier Outlook Calendar Booking

Make the appointment real in both systems: `zapier: Microsoft Outlook "Create Event"` (with an online-meeting/Teams link for remote sessions) on the tech's calendar, attendees invited, and the ticket's schedule mirrored in Thread so dispatch sees it too.

## When to use

- "Book a 30-minute remote session with <user> tomorrow afternoon."
- "Schedule the onsite at <client>'s office Thursday 9–11, add travel notes."
- A troubleshooting thread lands on "let's get on a call".

## Steps

1. Gather the essentials: who attends (tech via `search_members`, end user via `search_contacts` — addresses from records, not free-typed), duration, remote vs onsite, and the ticket.
2. Check for conflicts: `zapier: Microsoft Outlook "Find Events"` on the tech's calendar for the proposed window; offer alternatives on a clash rather than double-booking.
3. Compose the event: title "<client> — <purpose> (ticket <number>)"; body with the issue summary and what the user should have ready; for **remote**, enable the Teams meeting link on the event (note: Teams' own Zapier app cannot create meetings — Outlook Create Event with online meeting is the correct route); for **onsite**, put the site address in Location and any access instructions in the body.
4. Show event details + attendee list for confirmation, then `zapier: Microsoft Outlook "Create Event"` with attendees so invitations go out.
5. Mirror in Thread: `schedule_ticket` (or `update_schedule_entry` for a reschedule) so the dispatch view matches the calendar; `add_ticket_note` (plain text) with the booked time and meeting link reference.
6. If the tenant uses TimeZest and the member prefers self-scheduling, offer `create_timezest_scheduling_request` as the alternative — let the client pick the slot instead of proposing one.

## Guardrails

- **Member-connected Zapier required** (Outlook account with rights to the target calendar). Degrade to `schedule_ticket` in Thread plus a drafted invitation the member sends manually.
- Never book over an existing engagement without the tech's explicit okay; conflicts are surfaced, not steamrolled.
- Time zones: state the event time with its zone in the confirmation ("14:00 <site local time>"); mismatched zones are the classic booking failure. The end user's zone comes from their site/contact record.
- Invitations reach the client — the event body is client-facing text; no internal commentary.
- Reschedules edit the existing event and Thread schedule entry; never create a second event and orphan the first.
- Task cost: each Zapier call = 2 tasks; one Find Events + one Create Event per booking is the expected shape.
