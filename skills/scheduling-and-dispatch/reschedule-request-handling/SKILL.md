---
name: Reschedule Request Handling
description: A client asks to move an appointment — find the schedule entry or booking, propose new slots that actually work, update the entry, and confirm back to the client.
category: Scheduling & Dispatch
tools: [search_tickets, update_schedule_entry, schedule_ticket, add_ticket_note, get_timezest_scheduling_requests, create_timezest_scheduling_request]
---

# Reschedule Request Handling

Handle "can we move our appointment?" end to end: locate the existing commitment, offer real alternatives, move it once, and make sure both the ticket and the client reflect the new time.

## When to use

- A client replies "Thursday doesn't work anymore, can we do next week?"
- A tech asks to move a booked visit or session ("shift my 2pm to tomorrow").
- A conflict forces a change and the client needs new options.

## Steps

1. Find the commitment. From the ticket (`search_tickets` if needed), locate the existing schedule entry; also check `get_timezest_scheduling_requests` — if the appointment was TimeZest-booked, handle it in TimeZest terms rather than editing around it.
2. Confirm you have the right appointment: restate date, time, timezone, tech, and purpose. If multiple entries exist on the ticket, ask which one — never move the wrong appointment silently.
3. Get new availability for the same tech (calendar source if connected, else Thread schedule entries + load — see Calendar-Aware Scheduling) around the client's stated preference.
4. Propose 2–3 concrete alternative slots, timezone explicit, in a client-ready draft reply. If the appointment was TimeZest-booked, prefer sending a fresh scheduling link via `create_timezest_scheduling_request` so the client picks their own slot — after checking no open request already exists.
5. Once the client (or tech, for internal moves) confirms a slot: move the entry with `update_schedule_entry` (or `schedule_ticket` if the original entry was cancelled and a new one is needed). One move per confirmation — no speculative rebooking.
6. Record the change in a plain-text internal note via `add_ticket_note`: old time → new time, who requested it, and the reschedule count for this appointment.
7. Draft the confirmation reply with the new date/time/timezone for the tech to send.

## Guardrails

- Never move an appointment before the new time is confirmed by the requesting party; proposals are proposals.
- Restate the appointment being moved before touching it — wrong-entry edits are the failure mode here.
- Timezones explicit on every time you propose, book, or confirm.
- TimeZest-booked appointments: work through TimeZest (new request/link) rather than creating a parallel manual entry that disagrees with the booking system. Degradation: no TimeZest → manual slots and `update_schedule_entry` are the whole flow.
- Track repeat rescheduling in the note trail; if this is the third or more client-driven move, surface it to the tech (pattern worth a human conversation) — do not editorialize to the client.
- Client-facing messages are drafts for the tech to send; courteous, no blame, no internal details.
