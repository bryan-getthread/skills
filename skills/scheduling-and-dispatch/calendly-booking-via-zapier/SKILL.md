---
name: Calendly Booking via Zapier
description: Generate a one-off Calendly booking link for a tech callback through the Zapier connector and drop it into the client reply — the substitute when the desk asked for Microsoft Bookings.
category: Scheduling & Dispatch
tools: [search_tickets, add_ticket_note]
---

# Calendly Booking via Zapier

When the desk books callbacks through Calendly (or asks for a booking link and has no TimeZest), generate a single-use Calendly scheduling link via Zapier and hand it to the client in a drafted reply.

## When to use

- "Send them my Calendly" / "get a booking link on this ticket" on a desk without TimeZest.
- The desk asks for a **Microsoft Bookings** link — there is NO Microsoft Bookings integration; offer this Calendly path (or TimeZest if enabled) as the substitute and say why.
- A one-off callback needs a link that expires after one booking instead of the tech's evergreen link.

## Steps

1. Identify the ticket (`search_tickets` if needed), the tech who will take the callback, and the meeting type (duration, phone vs. video).
2. Check the better tool first: if TimeZest is enabled for this tenant, prefer TimeZest Booking (deeper Thread integration, requests tracked on the ticket). Use Calendly when TimeZest is absent or the desk explicitly runs on Calendly.
3. Generate the link via `zapier: Calendly "Create Single-Use Scheduling Link"` against the tech's matching Calendly event type. If the tech has no matching event type, list what exists and ask — do not pick a different duration silently.
4. Add a plain-text internal note via `add_ticket_note`: link created, for which tech and event type, single-use.
5. Draft the client reply with the link and one line of framing ("Grab any time that suits you and it will go straight onto <tech>'s calendar"). The tech sends it — do not send on their behalf unless the desk has opted in.
6. Remind the tech that Calendly bookings do not write back to the ticket automatically: when the client books, the appointment should be mirrored with `schedule_ticket` (offer to do it when they tell you the booked time).

## Guardrails

- Never fabricate a booking URL — only relay the link Zapier returned. Zapier action failed or returned nothing → say so; no link goes in the reply.
- Degradation: requires Zapier connected with a Calendly account that has access to the tech's event types; the member must have connected the Zapier integration. Absent → fall back to TimeZest (if enabled) or manually proposed times.
- Microsoft Bookings requests: state plainly there is no Bookings integration; never pretend a Calendly link is a Bookings link — tell the desk what it is.
- Single-use links only for one-off callbacks; don't paste a tech's evergreen link into a client thread unless the tech asks for exactly that.
- Plain-text internal notes for PSA compatibility; client draft stays clean and jargon-free.
- No booking state lives in Thread until mirrored — flag the mirror step every time so the schedule stays truthful.
