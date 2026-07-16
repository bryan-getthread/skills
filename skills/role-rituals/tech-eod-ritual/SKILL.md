---
name: Tech End-of-Day Ritual
description: A technician's close-of-day runbook — EOD wrap-up, time-entry compliance self-check, and tomorrow's first move — run when a tech says "wrap up my day", "EOD", or on a scheduled end-of-shift flow.
category: Role Rituals
tools: [search_tickets, log_time_entry]
---

# Tech End-of-Day Ritual

Closes a technician's day cleanly in ~10 minutes: wrap the queue, make sure the day's work is billed, and leave tomorrow-you a first move so the morning starts warm.

## When to use

- "Wrap up my day" / "run my EOD" / "am I good to log off?"
- End of shift, especially before a day off or handoff.
- A scheduled end-of-shift flow posting the wrap-up to the tech.

## Steps

1. **Wrap-up (5 min)** — run `skills/personal-productivity/eod-wrapup`: what closed today, what's still open with a client waiting, anything that must not sit overnight (candidates for `skills/escalation/shift-handoff` if after-hours coverage exists).
2. **Time-entry self-check (3 min)** — for every ticket touched today, verify a time entry exists (same lens as `skills/reporting-and-analytics/time-entry-compliance`, scoped to just me, just today). List gaps; offer to log the missing entries via `log_time_entry` with the tech's confirmation of durations — never guess hours.
3. **Tomorrow's first move (2 min)** — pick the single ticket tomorrow starts with and one line on why (SLA clock, promised callback, easiest unblock). Write it into the wrap-up output so morning-me (or `skills/role-rituals/tech-morning-ritual`) inherits it.
4. Output the EOD card: closed / still-hot / handed-off, time-entry gaps and what was logged, and tomorrow's first move.

## Time-short version

Step 2 (time gaps — the only thing that's hard to reconstruct later), the "must not sit overnight" line from step 1, and tomorrow's first move. Everything else can wait.

## Guardrails

- Never fabricate time entries — durations come from the tech, not from estimates. Unconfirmed gaps stay listed as gaps.
- Skip-with-note beats fake completion: a skipped step is printed as skipped with a reason.
- If something must not sit overnight and there is no coverage, say so explicitly rather than burying it in a list.
- Output goes to the tech's chat/flow destination; the only writes are time entries the tech confirmed.

## Unattended (Flows) variant

- Your entire reply is the posted EOD card, verbatim — no narration.
- Read-only: list time-entry gaps, never auto-log time (hours require human confirmation).
- Print all section headers even when empty. If searches fail, post what succeeded plus "data incomplete".
