---
name: Appointment Confirmation Email
description: Draft a confirmation for a scheduled visit or call — date, time, timezone, who's coming, what to expect, and how to reschedule. Use once an appointment is booked and the client needs written confirmation.
category: Communication
tools: [search_tickets, get_timezest_scheduling_requests, view_openDraft]
---

# Appointment Confirmation Email

Draft the confirmation a client receives after a visit or call is booked: the exact when and where, who will attend, what to expect, and the easy path to reschedule. It removes ambiguity and reduces no-shows.

## When to use

- "We booked the onsite for Thursday — send <client> the confirmation."
- "Confirm the scheduled call with the client."
- An appointment/visit has been scheduled and the client needs it in writing.

## Steps

1. Confirm the appointment details from the scheduling record (get_timezest_scheduling_requests where TimeZest is in use, or the ticket's schedule entry) before writing. Never state a date/time that isn't actually booked.
2. Draft the confirmation with the essentials unmissable:
   - **When** — date, start time, and **timezone** explicitly (and expected duration if recorded).
   - **Where / how** — onsite address or the call/meeting link/method.
   - **Who** — the technician(s) attending, if known.
   - **What to expect / prep** — anything the client should have ready or do beforehand, only if recorded.
   - **Reschedule path** — how to change or cancel (the booking link or "reply to this email").
3. Keep it short and skimmable.
4. Structure and tone per the Email Baseline Standard (communication/email-baseline-standard).
5. Present the draft with view_openDraft for review. Do not send.

## Guardrails

- **Only confirmed details.** Never fabricate a time, address, attendee, or duration. Timezone is mandatory — an ambiguous time causes missed appointments. Flag anything unknown as `NEEDS:`.
- **Don't create or move the booking here** — this skill confirms an existing appointment. Actual scheduling/rescheduling is a separate action (see scheduling skills below) and rescheduling requires the client's request.
- Degradation: view_openDraft and TimeZest tools are in-app only. Over external MCP, output the draft in chat labeled "DRAFT — review before sending" and take appointment details from the ticket.
- See scheduling-and-dispatch/timezest-booking and calendar-aware-scheduling for booking; reschedule-request-handling and appointment-noshow-followup for the before/after flow.
