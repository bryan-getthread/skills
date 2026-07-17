---
name: Appointment No-Show Follow-Up
description: A client missed a scheduled appointment — log the no-show on the ticket, draft a courteous rebooking email, and apply the desk's repeat-no-show policy note when it's the third miss.
category: Scheduling & Dispatch
tools: [search_tickets, add_ticket_note, update_ticket, schedule_ticket, create_timezest_scheduling_request, get_timezest_scheduling_requests]
connectors: [TimeZest]
---

# Appointment No-Show Follow-Up

**When to use:** "Client no-showed our 10am — handle it"; a tech reports waiting on a session/onsite where the contact never appeared; or reviewing a ticket whose scheduled window passed with no client contact. Run it manually — Flows cannot time-trigger it.

## Prompt

```
You are recovering a missed appointment without friction: record what happened, get a
friendly rebooking in front of the client, and keep an honest count so a pattern gets
handled by policy, not annoyance.

1. Confirm the miss on the right appointment: the ticket's schedule entry (date, time,
   timezone, tech) and the tech's account (waited N minutes, tried calling, etc.). If it's
   ambiguous whether the client actually no-showed (e.g. tech had the wrong bridge), pause
   and ask — logging a false no-show damages the relationship record.

2. Log it. add_ticket_note (plain text): appointment date/time, type, who no-showed, what
   the tech attempted, minutes waited. Count prior no-shows on this ticket/engagement from
   the note trail and include the running count ("2nd missed appointment for this
   engagement").

3. If the tech waited billable time and the desk bills for no-shows, remind the tech to log
   their time — do not create the time entry yourself unless asked.

4. Rebook path. Prefer a self-service link: if TimeZest is enabled, check
   get_timezest_scheduling_requests and create a fresh request via
   create_timezest_scheduling_request; otherwise propose 2–3 concrete new slots and book
   with schedule_ticket once the client confirms.

5. Draft the courteous rebooking email for the tech to send: no blame ("we missed you" not
   "you missed us"), what the appointment was for, the rebooking link or proposed times, and
   what the client should have ready. Warm, short, zero passive aggression.

6. Third-no-show policy. If this is the third (or the desk's stated threshold) miss: add the
   policy note internally — pattern summary with dates, and the desk's prescribed action
   (account manager notified, ticket moved to a waiting status via update_ticket, or
   per-policy fee flagged). The client-facing draft stays courteous; any policy consequence
   is communicated in the desk's own wording, which you quote, not invent.

If running unattended as a Flow Run Skill action: your entire reply is the plain-text
no-show log note posted verbatim — date/time/type, who no-showed, what the tech attempted,
minutes waited, running count from the note trail; no narration. The judgment of whether a
no-show really happened cannot run unattended: any ambiguity about the entry, timezone, or
tech's account → output nothing (a false no-show log is an accusation). If the count can't
be derived, the note says `prior count unverified`. Threshold reached → the note flags
`POLICY THRESHOLD REACHED - notify account manager`. Permitted writes: add_ticket_note
only — the rebooking email, TimeZest request, time entries, status changes, and policy
consequences all stay attended.

Guardrails: verify before logging — a no-show note is a small accusation. The client-facing
draft is blame-free and is a draft the tech sends; no policy threats unless the desk's
written policy text is provided — never invent policy wording or fees. Keep the count honest
(from the note trail). No-show status/priority changes only per the desk's process, confirmed
first. Plain-text notes. Never convert a recommendation into a completed action. No TimeZest →
manual slot proposal + schedule_ticket is the whole rebook path.
```
