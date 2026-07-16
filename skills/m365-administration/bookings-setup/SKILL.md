---
name: Bookings Setup
description: Configure Microsoft Bookings for a client — booking calendars, staff, services, availability, and booking-page/privacy settings. This is client-side Microsoft Bookings administration, NOT a Thread scheduling capability. Use when a client asks to set up online appointment booking, a self-service scheduling page, or staff/service calendars in Microsoft 365.
category: M365 Administration
tools: [search_tickets, search_clients, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, send_approval, log_time_entry, web_search]
---

# Bookings Setup

Sets up Microsoft Bookings inside a client's M365 tenant: the booking
calendar, staff, services, availability, and the public booking page.

> Scope note — read first. This is administering the client's Microsoft
> Bookings app. It is NOT a Thread scheduling feature and does NOT connect to
> Thread's own appointment tooling. For scheduling a Thread ticket/appointment,
> use the TimeZest scheduling tools instead — those are unrelated to Microsoft
> Bookings. Be explicit with the client about which they mean; "set up booking"
> is ambiguous.

## When to use

- "Set up an online booking page so clients can book appointments."
- "Add staff and services to Microsoft Bookings."
- "Change our Bookings availability / booking page."
- NOT for Thread ticket scheduling (use TimeZest via
  create_timezest_scheduling_request), and NOT for Teams meeting scheduling.

## Steps

1. Confirm it's Microsoft Bookings the client actually wants (not TimeZest,
   not Teams meetings, not a third-party scheduler) and that Bookings is
   available in their license — verify before building.
2. Booking calendar (business): create/confirm the Bookings calendar with the
   business name, logo, business hours, and time zone. One calendar per
   distinct booking front-end (e.g. a "New client consults" calendar).
3. Staff: add staff members with their role (team member / scheduler /
   viewer / guest) and their M365 account so availability syncs from their
   Outlook calendar. Decide whether each staff member's own calendar blocks
   Bookings availability (usually yes, to avoid double-booking). Adding staff
   who receive booking notifications is user-visible — confirm the list.
4. Services: define each bookable service — duration, buffer time before/after,
   price display, location (in-person / Teams online meeting), which staff can
   deliver it, and reminder settings. Map these to what the client described;
   don't invent services.
5. Availability + page settings: set availability rules (business hours,
   per-service, or per-staff), minimum lead time and maximum booking window,
   and the booking-page privacy — who can book (anyone with the link vs.
   internal only), whether staff pick is shown, and data-collection fields.
   Default to the more private option; a public "anyone" booking page is a
   deliberate choice, and only collect the customer data actually needed.
6. Approval: the booking page and staff notifications are client- and
   customer-facing. Get client sign-off (send_approval) on services,
   availability, staff list, and the page's public/private setting before it
   goes live.
7. Prepare execution + verify. Configuration is done in the Microsoft Bookings
   app (bookings.office.com / the Bookings web app); note where the client's
   own admin action is needed. Verify with a test booking: it lands on the
   right staff calendar, sends the confirmation/reminder, and respects buffers
   and lead time. Post a plain-text note: calendar, services (with durations/
   buffers), staff and roles, availability rules, page privacy, approver,
   date, and rollback (unpublish the booking page / remove services or staff).
   Log time.

## Guardrails

- This is Microsoft Bookings, not a Thread scheduling capability — say so; use
  TimeZest for Thread appointment scheduling. Do not conflate the two.
- Verify Bookings is licensed/available before promising it.
- Default the booking page to the more private option and collect only needed
  customer data; a public page is a deliberate, approved choice.
- Staff availability should honor their real calendars to prevent double-
  booking; confirm the staff and notification list with the client.
- Don't invent services or availability — mirror what the client specified.
  Capture the config so it can be unpublished/reverted.
