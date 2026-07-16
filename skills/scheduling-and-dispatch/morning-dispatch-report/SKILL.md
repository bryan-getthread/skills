---
name: Morning Dispatch Report
description: The dispatcher's start-of-day briefing — overnight arrivals, P1/P2 status, unassigned aging, today's scheduled work, and a top-10 priority work list; runnable unattended on a morning Flow.
category: Scheduling & Dispatch
tools: [search_tickets, list_boards, list_ticket_priorities, search_members]
---

# Morning Dispatch Report

Everything a dispatcher needs before the first assignment of the day, in one skimmable report: what came in overnight, what's urgent, what's aging, what's already booked, and the ten things to attack first.

## When to use

- "Morning dispatch report" / "what came in overnight?"
- A scheduled Flow posts the briefing before the desk opens each business day.
- A lead standing in for the dispatcher needs the same start-of-day picture.

## Steps

1. Resolve boards and priorities (`list_boards`, `list_ticket_priorities`). Define "overnight" as close of business yesterday to now, in the desk's timezone (use the desk's stated hours; default 6pm–now).
2. **Overnight arrivals.** New tickets created in the window, per board (one search per board — result caps apply). Group by priority, then client; call out clusters (several tickets from one client or one alert source usually mean one incident).
3. **P1/P2 status.** All open Critical and High tickets regardless of age: assignee, last activity, and whether the last word is the client's. Unassigned P1/P2s go at the very top of the report.
4. **Unassigned aging.** Open unassigned tickets by age bucket (<4h / 4–24h / >24h), flagging anything past the desk's thresholds.
5. **Today's schedule.** Scheduled entries for today by technician — onsites, booked sessions, blocks — so the dispatcher knows what capacity is already spoken for.
6. **Top-10 priority work list.** Rank open work by priority, age, client-response-waiting, and SLA pressure where visible; list the top 10 with a one-line "why it's here" each.
7. Output in that order with a two-line headline on top (e.g. "9 overnight, 1 Critical unassigned, 3 onsites today"). Compact tables, skimmable in under a minute.

## Guardrails

- Read-only: report and recommend; never assign or modify tickets from this skill.
- Result-cap honesty: if any per-board search may have capped, label that section's counts as "at least N".
- Aggregate, don't editorialize about people — this is a queue report, not a tech-performance report.
- Cite real ticket numbers only; never fabricate tickets, counts, or SLA states.
- If the overnight window or business hours are unstated and it matters, use the defaults and say which window was used.

## Running this unattended

> **Flows cannot schedule or time-trigger this.** Thread Flows fire on ticket *events* and conditions only — there is no schedule, cron, ticket-age, or elapsed-time trigger. This is a cadence/sweep skill, so run it **manually** on demand, or from an external scheduler that invokes Super Magic. A Flow can only reach it via **Run Skill** on a qualifying ticket event, never "every morning" or "after N hours". The output discipline below applies whenever it runs unattended.

- Your entire reply is the posted report — no narration, no questions, no preamble before the headline.
- Fixed section order and format every run, so readers can scan it on autopilot; include the date and the overnight window used.
- Keep formatting compatible with the destination: plain text if the Flow posts to a PSA-synced note.
- If a data pull fails or returns nothing, include the section with "no data / lookup failed" rather than omitting it silently.
- Never take assignment actions from this report run.
