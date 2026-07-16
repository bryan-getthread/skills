---
name: Tech Weekly Ritual
description: A technician's Friday self-review runbook — lite performance review, stale-ticket sweep of the tech's own queue, and one KB contribution pick — run when a tech says "weekly review", "clean up my queue for the week", or "how did my week go".
category: Role Rituals
tools: [search_tickets, search_knowledge_base]
---

# Tech Weekly Ritual

A ~30-minute weekly reset for an individual technician: see your own numbers, un-stick your own stale tickets, and give one thing back to the knowledge base.

## When to use

- "Run my weekly review" / "how did my week go" / "Friday cleanup."
- A tech prepping for their 1:1 who wants their own read first.
- End of the tech's work week, before the queue goes quiet for the weekend.

## Steps

1. **Performance review, lite (10 min)** — run `skills/personal-productivity/my-performance-review` scoped to this week only: closes, reopen count, response times, anything trending against me. This is a mirror, not a report card — no comparisons to teammates.
2. **Stale-ticket sweep, own queue (10 min)** — apply the `skills/qa-and-closure/stale-ticket-followup-cadence` lens to only my assigned tickets: anything with no activity beyond the cadence threshold. For each: follow up now, close with a proper note, or hand off — no ticket leaves the sweep without a decision.
3. **One KB contribution (10 min)** — run `skills/documentation/sop-candidate-finder` over my own resolved tickets this week and pick ONE resolution worth writing up. Draft it via `skills/documentation/kb-article-draft` if the tech has time; otherwise create a stub/reminder so the candidate isn't lost.
4. Output the weekly card: this week's numbers in 3–4 lines, stale tickets and the decision taken on each, and the KB pick (drafted or stubbed).

## Time-short version

Step 2 only (stale sweep — the one with client-facing consequences), plus one line of numbers from step 1. Skip the KB pick with a note; do not fake a contribution.

## Guardrails

- The ritual serves the tech: if the sweep uncovers a genuinely at-risk ticket, fixing it beats finishing the ritual.
- Self-review stays self — do not pull teammate comparisons into a personal ritual.
- One KB pick means one; a list of ten candidates nobody writes is worse than one written.
- Skip-with-note beats fake completion; every skipped step is printed with a reason.
- Stale-ticket follow-ups that message clients require the tech's confirmation before sending.
