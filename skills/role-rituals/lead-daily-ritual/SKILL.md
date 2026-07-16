---
name: Lead Daily Ritual
description: A service manager's daily runbook — leadership digest, escalation queue pass, silent-ticket sweep, and one coaching observation captured — run when a lead says "run my daily", "what needs me today", or on a scheduled weekday flow.
category: Role Rituals
tools: [search_tickets, search_members, list_boards]
---

# Lead Daily Ritual

The service manager's daily 20 minutes: see the desk, unblock what only a lead can unblock, catch the tickets going quiet, and bank one coaching note while it's fresh.

## When to use

- "Run my daily" / "what needs my attention today" / "lead morning pass."
- A team lead's first session of the day, after the huddle.
- A scheduled weekday flow posting the lead's daily picture.

## Steps

1. **Leadership digest (6 min)** — run `skills/reporting-and-analytics/daily-leadership-digest`: desk health, yesterday's anomalies, today's risks, anything trending wrong.
2. **Escalation queue (6 min)** — pull tickets escalated to or waiting on management (`search_tickets` on the escalation board/status). For each: decide, delegate, or set a checkpoint — a lead's escalation queue at standstill stalls everyone under it. Use `skills/escalation/management-escalation-brief` for any that need a structured read.
3. **Silent-ticket sweep (5 min)** — run `skills/qa-and-closure/silent-ticket-detector` across the team's boards: open tickets where the client is waiting and nothing has moved. Assign a nudge or an owner for each — this is the lead's daily defense against the tickets nobody notices.
4. **One coaching observation (3 min)** — from steps 1–3, capture exactly ONE specific, dated observation (good or corrective) about a specific tech into the lead's 1:1 notes destination (feeds `skills/reporting-and-analytics/one-on-one-prep`). Specific means ticket-referenced, not vibes.
5. Output the daily lead card: digest headline, escalation decisions made, silent tickets and who now owns each, and confirmation the coaching note was captured (not its contents, if the destination is private).

## Time-short version

Steps 2 and 3 — decisions only a lead can make, and the tickets going quietly bad. Digest and coaching note skip with a note.

## Guardrails

- The ritual serves the desk: an active incident preempts the ritual — go there, note the skip.
- Coaching observations are private-by-default: capture to the lead's notes destination, never into the team-visible output.
- One observation per day, tied to a real ticket — a daily habit of small specifics beats quarterly generalities, and fabricated praise is worse than none.
- Escalation decisions that message clients or reassign work follow the underlying skills' confirm-before-write gates.
- Skip-with-note beats fake completion; disclose result caps.

## Running this unattended

> **Flows cannot schedule or time-trigger this.** Thread Flows fire on ticket *events* and conditions only — there is no schedule, cron, ticket-age, or elapsed-time trigger. This is a cadence/sweep skill, so run it **manually** on demand, or from an external scheduler that invokes Super Magic. A Flow can only reach it via **Run Skill** on a qualifying ticket event, never "every morning" or "after N hours". The output discipline below applies whenever it runs unattended.

- Your entire reply is the posted lead card, verbatim — no narration.
- Read-only: list escalations awaiting decision and silent tickets; never decide, reassign, or write coaching notes autonomously.
- Omit the coaching-observation section entirely in unattended runs (it requires human judgment); print remaining headers even when empty.
- On search failure post what succeeded plus "data incomplete"; when in doubt, do nothing.
