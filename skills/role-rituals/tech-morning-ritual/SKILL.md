---
name: Tech Morning Ritual
description: A technician's 15-minute start-of-day runbook — digest, schedule check, easy-win pick, first-response sweep — run when a tech says "start my day", "what should I work on", or on a weekday-morning schedule.
category: Role Rituals
tools: [search_tickets, search_members, list_boards]
---

# Tech Morning Ritual

Orchestrates a technician's first 15 minutes: read the queue, know the calendar, bank one quick win, and make sure nobody is waiting on a first reply. Composes existing skills in a fixed order and ends with a committed first move.

## When to use

- "Start my day" / "what should I work on today" / "run my morning ritual."
- A tech logging in who wants the queue pre-digested before the huddle.
- A scheduled weekday-morning flow that posts the ritual output to the tech.

## Steps

Time-box the whole ritual to ~15 minutes. Each step names the skill it invokes.

1. **Digest (5 min)** — run `skills/personal-productivity/daily-digest`: open tickets, what needs replies, anything urgent, overnight arrivals on my queue.
2. **Schedule check (2 min)** — check today's scheduled ticket work and appointments (schedule entries via `search_tickets`; if the member's calendar connector is available, use it — otherwise ask the tech to confirm blocks). Flag any collision between scheduled work and urgent items from step 1.
3. **Easy-win pick (3 min)** — run `skills/personal-productivity/easy-win-finder` and pick exactly ONE quick-close candidate to do before lunch. One, not a list.
4. **First-response sweep (4 min)** — from the digest, isolate tickets in my queue with a client message and no tech reply yet. Oldest first. For each, either draft the reply now (offer `skills/communication/client-reply`) or explicitly defer with a reason.
5. **Commit (1 min)** — output the ritual card: today's calendar conflicts (if any), the one easy win, the first-response list with what was sent/deferred, and the single named "first move" the tech starts with right now.

## Time-short version

If the tech has under 5 minutes: step 1 (digest), step 4 (first-response sweep — client silence compounds fastest), and the one-line first move. Skip easy-win and calendar; note they were skipped.

## Guardrails

- The ritual serves the tech, not vice versa — if a live P1 is in the queue, stop the ritual at step 1 and point at the P1.
- Skip-with-note beats fake completion: any skipped step appears in the output as "skipped: <reason>", never silently dropped.
- Do not send client replies without the tech's confirmation in attended mode.
- Disclose result caps if the queue search may have truncated.
- Output lands in chat (or the flow's configured destination) — do not create tickets or notes as a side effect of a morning read.

## Running this unattended

> **Flows cannot schedule or time-trigger this.** Thread Flows fire on ticket *events* and conditions only — there is no schedule, cron, ticket-age, or elapsed-time trigger. This is a cadence/sweep skill, so run it **manually** on demand, or from an external scheduler that invokes Super Magic. A Flow can only reach it via **Run Skill** on a qualifying ticket event, never "every morning" or "after N hours". The output discipline below applies whenever it runs unattended.

- Your entire reply is the posted ritual card, verbatim — no narration, no questions.
- Steps 1–4 become read-only: list the first-response tickets, do NOT auto-send replies.
- Print every section header even when empty ("First-response: none waiting") so the format stays deterministic.
- If a search fails, post the sections that succeeded plus "data incomplete"; when in doubt, do nothing.
