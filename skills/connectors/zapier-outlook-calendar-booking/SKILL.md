---
name: Zapier Outlook Calendar Booking
description: Book remote sessions and onsite visits on the tech's Outlook calendar — Create Event with a Teams meeting link, attendees added, ticket referenced, and the ticket schedule mirrored in Thread. Use for "book a remote session with <user>", "schedule the onsite for Tuesday", or "put the follow-up call on my calendar".
category: Connectors
tools: [search_tickets, search_contacts, search_members, schedule_ticket, update_schedule_entry, add_ticket_note, create_timezest_scheduling_request]
connectors: [Zapier: Microsoft Outlook, TimeZest]
---

# Zapier Outlook Calendar Booking

**When to use:** "Book a 30-minute remote session with <user> tomorrow afternoon," or "schedule the onsite at <client>'s office Thursday 9–11."

## Prompt

```
Make the appointment real in both systems: zapier: Microsoft Outlook "Create Event" (with a
Teams/online-meeting link for remote sessions) on the tech's calendar, attendees invited, and the
ticket's schedule mirrored in Thread so dispatch sees it too.

This runs only if the member has connected Zapier with an Outlook account that has rights to the
target calendar. If Zapier is absent, degrade to schedule_ticket in Thread plus a drafted
invitation the member sends manually — do not stop. Each Zapier call = 2 Zapier tasks.

1. Gather essentials: who attends (tech via search_members, end user via search_contacts —
   addresses from records, not free-typed), duration, remote vs onsite, and the ticket.
2. Check for conflicts: "Find Events" on the tech's calendar for the proposed window; offer
   alternatives on a clash rather than double-booking. Never book over an existing engagement
   without the tech's explicit okay.
3. Compose the event: title "<client> — <purpose> (ticket <number>)"; body with the issue summary
   and what the user should have ready. For remote, enable the Teams/online meeting link on the
   event (Teams' own Zapier app cannot create meetings — Outlook Create Event with an online meeting
   is the correct route). For onsite, put the site address in Location and access instructions in
   the body. State the event time with its zone ("14:00 <site local time>"); the end user's zone
   comes from their site/contact record.
4. Show me event details + attendee list, wait for confirmation, then "Create Event" with attendees
   so invitations go out. The event body is client-facing — no internal commentary.
5. Mirror in Thread: schedule_ticket (or update_schedule_entry for a reschedule) so dispatch matches
   the calendar; add_ticket_note (plain text) with the booked time and meeting-link reference.
   Reschedules edit the existing event and schedule entry — never create a second event and orphan
   the first.
6. If the tenant uses TimeZest and the member prefers self-scheduling, offer
   create_timezest_scheduling_request instead — let the client pick the slot.
```
