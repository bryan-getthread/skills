---
name: Calendar-Aware Scheduling
description: Put ticket work on a technician's schedule around their real calendar — check busy periods before proposing a slot, then book it with schedule_ticket.
category: Scheduling & Dispatch
tools: [search_tickets, search_members, schedule_ticket, update_schedule_entry]
---

# Calendar-Aware Scheduling

Schedule ticket work into slots the technician can actually make: read real calendar availability where a calendar source is connected, propose a slot, and book it on the ticket.

## When to use

- "Schedule this ticket for <tech> tomorrow" / "find time for this on <tech>'s calendar".
- "When can we do this migration? Book it."
- Rebooking work after a conflict ("move my 2pm ticket to when I'm actually free").

## Steps

1. Resolve the ticket and the technician (`search_tickets`, `search_members`), and the desired window ("tomorrow morning", "this week", a duration if stated — default to 1 hour if not).
2. **Check real availability, best source first:**
   - If the member has connected their Microsoft 365 account, read their Outlook calendar for the window and collect busy periods.
   - Else if Zapier is connected with Google Calendar: `zapier: Google Calendar "Find Busy Periods"` for the tech's calendar over the window.
   - Else fall back to Thread-visible commitments only: existing schedule entries and open ticket load — and say the view excludes their external calendar.
3. Also pull the tech's existing Thread schedule entries for the window so you don't double-book against ticket work that isn't on the external calendar.
4. Propose 1–3 candidate slots that avoid all known busy periods, respecting working hours (default business hours in the tech's timezone unless the desk says otherwise). Show what each slot avoids ("2–3pm: clear on Outlook, no schedule entries").
5. On confirmation, book with `schedule_ticket` (or `update_schedule_entry` when moving an existing entry). Confirm back with the exact date, time, and timezone.
6. If the client must attend, offer to draft the client-facing confirmation message with the booked time — draft only; the tech sends it.

## Guardrails

- Never present a slot as "free" unless a calendar source confirmed it; label fallback proposals as "not checked against their external calendar".
- Time zones explicitly, always — state the timezone on every proposed and booked slot; client and tech may differ.
- Confirm before booking, and before moving any existing schedule entry.
- Degradation: calendar reads require the member to have connected Microsoft 365, or Zapier with Google Calendar. Neither connected → fall back to Thread schedule entries + load, and say so.
- Do not create calendar events in external calendars from this skill — book via `schedule_ticket` so the ticket stays the source of truth; external-calendar sync is the integration's job.
- When no slot fits the requested window, say so and propose the nearest alternatives — never silently book outside the asked window.
