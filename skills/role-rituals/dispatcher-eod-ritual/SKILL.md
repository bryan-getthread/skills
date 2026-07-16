---
name: Dispatcher End-of-Day Ritual
description: A dispatcher's close-of-board runbook — after-hours handoff prep, tomorrow's schedule sanity check, and an unassigned-zero check — run when a dispatcher says "close the board", "EOD dispatch", or on a scheduled end-of-day flow.
category: Role Rituals
tools: [search_tickets, search_members, list_boards]
---

# Dispatcher End-of-Day Ritual

Ends the dispatch day so the night and the morning both start clean: after-hours coverage knows what's hot, tomorrow's schedule survives contact with reality, and nothing is left ownerless overnight.

## When to use

- "Close the board" / "run EOD dispatch" / "prep the after-hours handoff."
- The coordinator's last pass before leaving.
- A scheduled end-of-day flow posting the handoff picture.

## Steps

1. **After-hours handoff (8 min)** — run `skills/scheduling-and-dispatch/after-hours-coverage-handoff`: open P1/P2s with current state, promised callbacks, anything a night tech could get blindsided by. This is the step that cannot be skipped.
2. **Tomorrow's schedule sanity (5 min)** — check tomorrow's scheduled tickets and appointments against tomorrow's roster (`skills/scheduling-and-dispatch/technician-availability-check`): double-bookings, work scheduled on someone who's out, field visits with no travel buffer. Fix or flag each.
3. **Unassigned zero check (2 min)** — one final `search_tickets` for unassigned open tickets. Anything that arrived since the morning ritual gets an owner now or is explicitly placed in the after-hours handoff with a reason.
4. Output the closing card: handoff summary (or link to where it was posted), tomorrow's schedule exceptions and fixes, and "unassigned: 0" or the named exceptions.

## Time-short version

Step 1 (handoff) and step 3 (unassigned zero). Tomorrow's schedule can be sanity-checked in the morning ritual — note the skip so morning-dispatch picks it up.

## Guardrails

- The handoff is for the night tech's benefit: lead with what's hot, not with volume stats.
- Never mark unassigned "zero" by assigning to people who've left for the day — after-hours items go to actual coverage or into the handoff.
- Schedule fixes that move client appointments require confirmation (and client notice via the proper skill) — flag, don't silently move.
- Skip-with-note beats fake completion; disclose result caps.

## Running this unattended

> **Flows cannot schedule or time-trigger this.** Thread Flows fire on ticket *events* and conditions only — there is no schedule, cron, ticket-age, or elapsed-time trigger. This is a cadence/sweep skill, so run it **manually** on demand, or from an external scheduler that invokes Super Magic. A Flow can only reach it via **Run Skill** on a qualifying ticket event, never "every morning" or "after N hours". The output discipline below applies whenever it runs unattended.

- Your entire reply is the posted closing card, verbatim — no narration.
- Read-only: list unassigned tickets and schedule conflicts; never auto-reassign or move appointments after hours.
- Print all sections even when empty; on search failure post what succeeded plus "data incomplete".
