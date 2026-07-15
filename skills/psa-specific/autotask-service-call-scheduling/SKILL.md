---
name: Autotask Service Call Scheduling
description: For desks synced to Autotask — know when work is a Service Call (a scheduled visit/appointment object) versus just a ticket, and follow Autotask scheduling conventions so dispatch calendars stay true.
category: PSA-Specific
tools: [search_tickets, schedule_ticket, update_schedule_entry, add_ticket_note, search_members, search_contacts]
---

# Autotask Service Call Scheduling

Applies to: **Autotask (Datto/Kaseya)**. Autotask separates the *ticket* (the work record) from the *Service Call* (a scheduled block of time, possibly covering several tickets, that drives the dispatch calendar and onsite logistics). Desks that schedule by dropping a due date on the ticket instead of creating the scheduled entry end up with invisible commitments and double-booked techs.

## When to use

- Scheduling onsite or appointment-based work on an Autotask desk.
- "Put this on the calendar" / a client agreed to a visit window.
- Auditing tickets that promise a visit in the thread but show nothing scheduled.

## Steps

1. Re-fetch the ticket with `search_tickets` at full detail — confirm it is open and that no schedule entry already exists (double-booking the same visit is the most common failure).
2. Decide if this is service-call-shaped work: onsite visit, fixed appointment with the client, or a time-blocked remote session. Routine queue work does not get a schedule entry — scheduling everything destroys the calendar's signal.
3. Gather the scheduling facts before writing: confirmed date/window with the client (from the thread — never assume a proposed time was accepted), duration estimate, assigned resource (`search_members`), onsite contact and site details (`search_contacts`). Missing confirmation → chase it via `skills/scheduling-and-dispatch/field-visit-scheduler` or TimeZest (`skills/scheduling-and-dispatch/timezest-booking`) rather than penciling in a guess.
4. Create the entry with `schedule_ticket` (or amend with `update_schedule_entry` for reschedules). Per Autotask convention, one schedule entry per discrete visit; if one visit covers several tickets, schedule the primary ticket and cross-reference the others in notes.
5. Add a plain-text `add_ticket_note` stating the scheduled window, resource, and onsite contact — this is what the client-facing record and the next tech read.
6. Output: what was scheduled (or proposed), for whom, evidence the client confirmed the window, and any reschedule trail. Reschedules always note the old window and the reason.

## Guardrails

- **Never schedule an unconfirmed time.** A proposed window in the thread is not an agreement; scheduling it fabricates a commitment.
- **Sync lag:** re-fetch full ticket detail before creating or moving a schedule entry — the visit may already be scheduled, completed, or the ticket closed in Autotask.
- One visit, one entry; never stack duplicate entries to "block extra time" — extend the duration instead.
- Reschedules edit the existing entry (`update_schedule_entry`); creating a second entry leaves a ghost booking on the dispatch calendar.
- Time worked is still logged separately (`log_time_entry` conventions per the desk); a schedule entry is a plan, not a time record — never treat scheduled hours as worked hours.
- Degradation: if scheduling tools are unavailable for this tenant, output the complete scheduling packet (window, resource, contact, site) as a note for a human dispatcher instead.

## Cross-references

- Logistics and confirmation loop: `skills/scheduling-and-dispatch/field-visit-scheduler`, `skills/scheduling-and-dispatch/appointment-noshow-followup`
- Client-driven booking: `skills/scheduling-and-dispatch/timezest-booking`
