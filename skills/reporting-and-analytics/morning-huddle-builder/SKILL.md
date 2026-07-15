---
name: Morning Huddle Builder
description: Build the daily standup or morning huddle message — yesterday's P1s, overnight items, today's SLA risks, and shout-outs — ready to read out or paste.
category: Reporting & Analytics
tools: [search_tickets, search_members, list_boards]
---

# Morning Huddle Builder

Assemble a structured daily standup message the team can absorb in under a minute: what happened yesterday, what came in overnight, what is at risk today, and who deserves a shout-out.

## When to use

- "Build this morning's huddle" / "prep the daily standup."
- A dispatcher or lead running the 8am sync who wants the queue read pre-digested.
- A scheduled flow that posts the huddle each weekday morning.

## Steps

1. Establish the windows: "yesterday" = the previous business day; "overnight" = close of business to now. Exclude junk/NOC noise boards.
2. Run split searches per signal (disclose caps):
   - **Yesterday's P1s/majors** — status now, one line each.
   - **Overnight arrivals** — new tickets since close, flagging anything urgent or unassigned.
   - **Today's SLA risks** — tickets whose response or resolution clock lands today.
   - **Shout-outs** — 1–2 genuinely notable saves from yesterday (tough resolution, great client reply, big backlog dent).
3. Format as a fixed skimmable structure:
   - **Yesterday:** P1 recap in 1–2 lines each.
   - **Overnight:** count + the items needing an owner first thing.
   - **Today's risks:** SLA-clock tickets with owner and deadline.
   - **Shout-out:** one or two, specific and genuine.
4. Keep the whole message under ~15 lines. Every item names an owner or says "needs an owner."

## Guardrails

- Unassigned urgent items go at the top — the huddle's job is to get them owned, not buried.
- Shout-outs must cite something real from yesterday's tickets; skip the section entirely rather than fabricate praise.
- No client-blaming language — this message often ends up screen-shared.
- Disclose result caps in a trailing note if overnight volume exceeded a search.
- Do not include sensitive content (credentials, security-incident details) — reference the ticket instead.

## Unattended (Flows) variant

- Your entire reply is the posted huddle message, verbatim — no narration, no questions.
- If a section has nothing to report, print the header with "none" rather than omitting it, so the format stays deterministic.
- If searches fail, post only the sections that succeeded plus a "data incomplete" line; never fill gaps with guesses.
