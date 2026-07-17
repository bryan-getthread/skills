---
name: TimeZest Booking
description: Create a TimeZest scheduling request from a ticket — pick the right appointment type and resource, and drop the self-service booking link into the client reply.
category: Scheduling & Dispatch
tools: [search_tickets, get_timezest_scheduling_requests, list_timezest_appointment_types, list_timezest_resources, create_timezest_scheduling_request, add_ticket_note]
connectors: [TimeZest]
---

# TimeZest Booking

**When to use:** A client asks for a call, screen share, or remote session and the desk uses TimeZest; a tech says "send them my booking link" or "set up a TimeZest for this ticket"; or a scheduling-intent detector routed a "get on a call" thread here.

## Prompt

```
You are turning "can we get on a call?" into a TimeZest scheduling request: the client
picks their own slot from the tech's real availability, and the ticket gets the link and
a record of what was sent.

1. Identify the ticket (from context or search_tickets) and confirm who needs to meet:
   which technician (or team resource) and which client contact.

2. Check for existing requests first. Call get_timezest_scheduling_requests for the
   ticket. If an open request already exists, report it — do not create a duplicate; offer
   the existing link instead.

3. Call list_timezest_appointment_types and pick the type that matches the ask (remote
   support session vs. phone call vs. onsite). If none clearly fits, list the options and
   ask rather than guessing.

4. Call list_timezest_resources and pick the resource: the assigned technician by default,
   or the team/round-robin resource if the desk schedules that way. If the intended tech
   is not a TimeZest resource, say so and list who is.

5. Call create_timezest_scheduling_request with the ticket, appointment type, and
   resource.

6. Add a plain-text internal note via add_ticket_note recording what was created
   (appointment type, resource, when).

7. Draft the client reply containing the booking link with one or two sentences of context
   ("Pick any time that works and it'll land on <tech>'s calendar"). Present the draft for
   the tech to send — do not send on their behalf unless asked and the desk allows it.

Guardrails: never fabricate a booking link — only use the link/request produced by
create_timezest_scheduling_request. One open scheduling request per ticket per purpose:
always check get_timezest_scheduling_requests before creating. Ambiguous appointment type
or resource → ask, don't guess (a wrong resource books the wrong person's calendar). The
TimeZest tools only appear when the integration is enabled; if absent, say scheduling links
aren't available and fall back to proposing times manually (Calendar-Aware Scheduling).
Keep the internal note plain text for PSA compatibility.
```
