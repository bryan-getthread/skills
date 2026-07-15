---
name: Scheduling Intent Detector
description: Find "can we get on a call / book a time" requests hiding in recent tickets and route them to scheduling — skipping tickets where a tech has already engaged.
category: Triage & Routing
tools: [search_tickets, add_ticket_note, update_ticket, create_timezest_scheduling_request, list_timezest_appointment_types, list_timezest_resources]
---

# Scheduling Intent Detector

Clients ask for calls in the middle of ticket threads and the ask gets buried. This skill scans recent tickets for scheduling intent and routes those tickets into the desk's scheduling path before they go stale.

## When to use

- Periodic sweep: "find tickets where the client asked to get on a call."
- A flow checks new client messages for scheduling requests.
- Dispatch wants a morning list of unactioned booking asks.

## Steps

1. Search recent tickets with `search_tickets` within the window (default: 48 hours), scoped per board; disclose result caps and split queries per board when scanning widely.
2. In each thread's latest client messages, detect scheduling intent: explicit asks to book a call/meeting/visit, "when are you available", proposed times, or requests to reschedule an existing appointment. The ask must come from the client, not the tech.
3. Skip tickets with prior tech engagement on the ask: if a tech has replied since the scheduling request, already proposed times, or a scheduling entry/appointment already exists, leave the ticket alone.
4. For each live, unactioned ask: if TimeZest is connected, create the booking via `create_timezest_scheduling_request` using an appointment type from `list_timezest_appointment_types` and a resource from `list_timezest_resources` per the desk's mapping. If TimeZest is not available for this tenant, instead move the ticket to the scheduling/dispatch path per desk convention (`update_ticket`) — degrade gracefully, do not fail silently.
5. Post a plain-text note on each actioned ticket quoting the client's ask and stating what was routed where.
6. Output a summary list: ticket number, client, the quoted ask, action taken or skipped-with-reason.

## Guardrails

- Only client-authored messages count as scheduling intent; a tech saying "we should schedule" is not a trigger.
- Prior tech work on the ask → skip; never double-book or send a second scheduling request for the same ask.
- Choose appointment types and resources only from the configured mapping; if the ask does not map cleanly (unknown duration, unclear attendee), flag instead of booking.
- Quote the ask verbatim in the note — do not paraphrase times or commitments.
- Degradation: without TimeZest, this skill routes and flags; it never fabricates booking links.

## Unattended (Flows) variant

- Your entire reply is the internal note on the single triggering ticket, verbatim: `SCHEDULING INTENT: "<quoted ask>". Action: <scheduling request created / routed to scheduling / skipped - tech engaged>.`
- One action per ticket per run; if a note from this skill already covers the same ask, stop.
- Ambiguous intent (venting vs. actually requesting a call) → skip with reason. When in doubt, do nothing.
