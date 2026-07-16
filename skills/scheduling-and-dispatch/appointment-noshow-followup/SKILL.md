---
name: Appointment No-Show Follow-Up
description: A client missed a scheduled appointment — log the no-show on the ticket, draft a courteous rebooking email, and apply the desk's repeat-no-show policy note when it's the third miss.
category: Scheduling & Dispatch
tools: [search_tickets, add_ticket_note, update_ticket, schedule_ticket, create_timezest_scheduling_request, get_timezest_scheduling_requests]
---

# Appointment No-Show Follow-Up

Recover a missed appointment without friction: record what happened, get a friendly rebooking in front of the client, and keep an honest count so a pattern gets handled by policy, not annoyance.

## When to use

- "Client no-showed our 10am — handle it."
- A tech reports waiting on a session/onsite where the contact never appeared.
- Reviewing a ticket whose scheduled window passed with no client contact.

## Steps

1. Confirm the miss on the right appointment: the ticket's schedule entry (date, time, timezone, tech) and the tech's account of what happened (waited N minutes, tried calling, etc.). If it's ambiguous whether the client actually no-showed (e.g. tech may have had the wrong bridge), pause and ask — logging a false no-show damages the relationship record.
2. **Log it.** `add_ticket_note` (plain text): appointment date/time, appointment type, who no-showed, what the tech attempted (calls, messages), and minutes waited. Count prior no-shows on this ticket/engagement from the note trail and include the running count ("2nd missed appointment for this engagement").
3. If the tech waited billable time and the desk bills for no-shows, remind the tech to log their time — do not create the time entry yourself unless asked (zero-assumption rule).
4. **Rebook path.** Prefer a self-service link: if TimeZest is enabled, check `get_timezest_scheduling_requests` and create a fresh request via `create_timezest_scheduling_request`; otherwise propose 2–3 concrete new slots and book with `schedule_ticket` once the client confirms.
5. **Draft the courteous rebooking email** for the tech to send: no blame ("we missed you" not "you missed us"), what the appointment was for, the rebooking link or proposed times, and what the client should have ready. Warm, short, zero passive aggression.
6. **Third-no-show policy.** If this is the third (or the desk's stated threshold) missed appointment: add the policy note internally — pattern summary with dates, and the desk's prescribed action (e.g. account manager notified, ticket moved to a waiting status via `update_ticket`, or per-policy fee flagged). The client-facing draft stays courteous; any policy consequence is communicated in the desk's own wording, which you quote, not invent.

## Guardrails

- Verify before logging: a no-show note is a small accusation — confirm the tech's account and the right schedule entry first.
- The client-facing draft is blame-free and is a draft — the tech sends it. No policy threats in client email unless the desk's written policy text is provided; never invent policy wording or fees.
- Keep the count honest: derive it from the note trail, state it in the note; never inflate or estimate.
- No-show status/priority changes only per the desk's process, confirmed before applying.
- Plain-text notes for PSA compatibility.
- Never convert a recommendation into a completed action: suggest the time entry and the AM escalation; do them only on confirmation.
- Degradation: no TimeZest → manual slot proposal + `schedule_ticket` is the whole rebook path (or Calendly via Zapier if that's the desk's tool).

## Running this unattended

> **Flows cannot schedule or time-trigger this.** Thread Flows fire on ticket *events* and conditions only — there is no schedule, cron, ticket-age, or elapsed-time trigger. This is a cadence/sweep skill, so run it **manually** on demand, or from an external scheduler that invokes Super Magic. A Flow can only reach it via **Run Skill** on a qualifying ticket event, never "every morning" or "after N hours". The output discipline below applies whenever it runs unattended.

- Follows the Unattended Output Discipline contract: the entire reply is the plain-text no-show log note posted verbatim — appointment date/time/type, who no-showed, what the tech attempted, minutes waited, and the running count derived from the note trail. No narration.
- Deterministic inputs from the flow: the ticket id, the schedule entry, and a confirmed no-show event (the tech marked it, or the scheduling tool reported it). The judgment of whether a no-show really happened cannot run unattended: any ambiguity about the entry, the timezone, or the tech's account → output nothing. A false no-show log is an accusation.
- The running count comes strictly from prior notes on the trail; if it cannot be derived, the note says `prior count unverified` instead of a number.
- Threshold reached (third miss or the desk's stated number) → the note flags `POLICY THRESHOLD REACHED - notify account manager`; the consequence itself is a human action.
- Permitted writes: `add_ticket_note` only. The rebooking email, TimeZest scheduling request (it reaches the client), time entries, status changes, and policy consequences all stay attended.
