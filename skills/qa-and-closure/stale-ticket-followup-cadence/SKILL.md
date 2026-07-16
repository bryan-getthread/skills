---
name: Stale Ticket Follow-Up Cadence
description: Drive a configurable follow-up cadence (for example 24/48/72 hours, three attempts) on tickets waiting for a client reply — draft each friendly nudge, count documented attempts, and skip tickets in legitimate waits.
category: QA & Closure
tools: [search_tickets, add_ticket_note, update_ticket, schedule_ticket]
---

# Stale Ticket Follow-Up Cadence

Keeps waiting-on-client tickets moving on a predictable rhythm. Counts the attempts already documented in the thread, drafts the next follow-up in a friendly client-facing voice, and knows when a ticket is waiting for a good reason and should be left alone.

## When to use

- "Follow up on my waiting tickets" / "who hasn't replied to us?"
- Running the desk's standard cadence (24/48/72 hours, or the house variant) across a queue.
- A tech wants the next nudge drafted for one specific quiet ticket.
- Embedded in a Flow on a schedule or on a waiting-status timer (see Unattended variant).

## Steps

1. Confirm the cadence to apply. Default: first follow-up 24 hours after the last outbound client-facing message, second at 48, third at 72 — three attempts total, then hand off to the No-Response Closure Sequence skill. Honor a desk-specific cadence if the user states one ("3 days / 3 attempts / 3 emails" is a common house rule).
2. Find candidates with `search_tickets`: open tickets in waiting-on-client statuses where the last message is outbound and older than the current cadence step.
3. For each candidate, count **documented attempts**: client-visible follow-up messages since the client last replied. Only messages the client could actually see count — internal notes are not attempts.
4. Skip legitimate waits: a future scheduled appointment (`schedule_ticket` date), a client-stated timeline ("back from vacation Monday"), a vendor/third-party dependency, or an approval pending. Do not nudge someone who told you when they will respond.
5. Draft the follow-up for each remaining ticket in the client-facing voice: friendly, short, references the original issue in one line, states what is needed from them, and offers an easy out ("if this is resolved, let us know and we will close it"). Escalate tone gently across attempts — attempt three notes that the ticket will be closed soon without a reply, and never sounds annoyed.
6. Present drafts for review, then post each approved follow-up as a client-facing note with `add_ticket_note` and set the appropriate waiting status with `update_ticket`.
7. Output a summary table: ticket, client, attempt number just sent (or skipped and why), and the date the next cadence step falls due.

## Guardrails

- Never exceed the attempt count: if the thread already shows three documented attempts, route to closure sequence instead of nudging a fourth time.
- Count only what is documented — never assume an attempt happened by phone unless a note records it.
- Client-facing drafts contain no internal jargon, no ticket-system boilerplate, and no invented details about the issue.
- Skip-list is sacred: a wrongly nudged client on vacation or awaiting a scheduled visit erodes trust. When wait legitimacy is unclear, skip and flag rather than send.
- Never bulk-send follow-ups without the user reviewing the drafts.
- Keep language simple and localizable; match the client's language where the thread is not in English.

## Running this unattended

> **Flows cannot schedule or time-trigger this.** Thread Flows fire on ticket *events* and conditions only — there is no schedule, cron, ticket-age, or elapsed-time trigger. This is a cadence/sweep skill, so run it **manually** on demand, or from an external scheduler that invokes Super Magic. A Flow can only reach it via **Run Skill** on a qualifying ticket event, never "every morning" or "after N hours". The output discipline below applies whenever it runs unattended.

- Trigger: schedule (e.g. hourly sweep) or waiting-status age timer.
- Your entire reply is the client-facing follow-up message, posted verbatim — no narration, no attempt counters, no internal commentary. Write nothing else.
- Deterministic rules: send only when (a) the ticket is in a waiting-on-client status, (b) the last message is outbound, (c) the cadence step is due, and (d) documented attempts are below the maximum. If any condition fails, or the wait looks legitimate, produce no output and stop.
- Never draft attempt three or beyond unattended if the desk requires human sign-off before final notices — check the configured attempt ceiling and stop below it.
