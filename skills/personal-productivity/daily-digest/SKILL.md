---
name: Daily Digest
description: A technician asks for a summary of their open tickets — what needs a reply, what's urgent, what's scheduled today — skimmable in under a minute, with a 3-line ultra-short option.
category: Personal Productivity
tools: [search_tickets, search_members]
---

# Daily Digest

The morning read: everything on your plate, sorted into what needs you now. Built to be skimmed in under a minute and to name the single first thing to do. Also the pattern most partners run as a scheduled morning skill.

## When to use

- "Give me a summary of my open tickets" / "what needs replies, anything urgent?"
- "Morning digest" / "what's my day look like?"
- "Short version" — the 3-line variant
- Embedded in a Flow as a scheduled start-of-day briefing

## Steps

1. Pull all open tickets assigned to the requesting member with search_tickets. If a result cap may have truncated the list, say so up front ("showing 50 — there may be more") instead of presenting the digest as complete.
2. Sort every ticket into exactly one bucket, in this priority order (a ticket lands in the first bucket it qualifies for):
   - **Needs your reply** — the client responded last and is waiting on you. Order by how long they've been waiting.
   - **Urgent / at risk** — high priority, SLA at or near breach, or negative-sentiment threads. Order by severity.
   - **Scheduled today** — tickets with a schedule entry or committed follow-up today, in time order.
   - **Waiting on others** — client, vendor, or escalation has the ball. Flag any that have been quiet 3+ days (a nudge candidate), but don't let this bucket crowd the top.
   - **Everything else** — count and a one-line characterization only; no per-ticket lines.
3. Each ticket line is one line: number, client (short), 5–8-word state, and the next action ("#1234 <client> — printer offline 2d, client replied yesterday → confirm fix worked").
4. Lead the digest with **Start here:** the single most important ticket and one sentence on why (longest-waiting client reply beats vague urgency; hard SLA breach beats everything).
5. Close with the shape of the day: totals per bucket and one sentence ("2 replies and 1 urgent before your 10:00 scheduled visit clears most of the pressure").
6. If asked for the short version (or "3 lines"), output exactly three lines: (1) Start here + why, (2) counts — X need replies / Y urgent / Z scheduled, (3) the one thing that will bite if ignored today. No headers, no preamble.
7. Offer the natural follow-up: "Want reply drafts for the ones waiting on you?"

## Guardrails

- Scope strictly to the requesting member's own tickets; never include other techs' queues unless explicitly asked (that's a lead's report, not a digest).
- Read-only: the digest changes nothing — no status updates, no notes, no reminders — unless the member asks as a follow-up.
- Never invent ticket numbers, SLA times, or client replies; if last-activity data is ambiguous, put the ticket in the safer bucket (Needs your reply) rather than hiding it in Waiting.
- Every bucket line must be actionable — a state without a next action is a line wasted.
- Keep the full digest skimmable: if it doesn't fit on one screen, tighten the lines; never pad.

## Running this unattended

> **Flows cannot schedule or time-trigger this.** Thread Flows fire on ticket *events* and conditions only — there is no schedule, cron, ticket-age, or elapsed-time trigger. This is a cadence/sweep skill, so run it **manually** on demand, or from an external scheduler that invokes Super Magic. A Flow can only reach it via **Run Skill** on a qualifying ticket event, never "every morning" or "after N hours". The output discipline below applies whenever it runs unattended.

- Your entire reply is delivered verbatim as the morning briefing — no narration, no questions, no "let me know if".
- Always include the Start here line and the bucket counts; omit the follow-up offer.
- If the queue is empty, output exactly: "No open tickets assigned to you. Enjoy the clean slate."
- If the ticket search fails or returns implausibly empty for an active tech, output a single line saying the digest could not be generated — never a fabricated digest.
