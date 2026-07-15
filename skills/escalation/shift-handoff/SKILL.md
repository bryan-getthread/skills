---
name: Shift Handoff
description: Produce a skimmable end-of-shift or end-of-day one-pager of open work, grouped by status, for the next shift or a single named receiving technician.
category: Escalation
tools: [search_tickets, add_ticket_note, log_time_entry, search_members]
---

# Shift Handoff

Builds a one-minute-read handoff one-pager: every open thread grouped by status, each with what was done, what's next, what it's waiting on, and its risk. Scopes to a single receiving technician when one is named.

## When to use

- End of shift or end of day: "hand off my open work to the night shift."
- "Write up a handoff for <member> — they're covering my queue tomorrow."
- A lead wants the team's open work summarized before a shift change.

## Steps

1. Establish scope. If a single receiving technician is named, hand off only what that person needs to act on (the outgoing tech's open/in-progress tickets, plus anything explicitly flagged for them) — not the whole team's queue. Otherwise cover the team's open work for the incoming shift.
2. Pull open and in-progress tickets via search_tickets. If any search hits a result cap, say so in the output rather than presenting the list as complete; split searches per board or per status when scanning broadly.
3. Group the one-pager by status (e.g. In Progress / Waiting on Client / Waiting on Vendor / Scheduled / New-untouched), so the reader sees at a glance what is actionable versus parked.
4. For each thread, use the fixed four-line entry: **Work** (what was done this shift), **Next** (the single next action and its owner), **Waiting** (who or what it's blocked on, and since when), **Risk** (SLA proximity, sentiment, VIP, or "none").
5. Top the page with a callout section: anything time-critical in the next few hours, tickets approaching SLA, and promised callbacks.
6. For end of day, offer to log outstanding time via log_time_entry and to post per-ticket handoff notes (plain text) for anything unresolved — post only on confirmation.

## Guardrails

- Optimize for read speed: the receiver should know what to pick up in under a minute. Aggregate; don't paste ticket threads.
- Never omit blockers or SLA-risk items to keep the summary short.
- Disclose result caps — do not present a capped search as the full queue.
- Ticket notes are plain text; the one-pager itself is chat output unless asked otherwise.
- For transferring a live, in-progress call, use the Live Call Transfer Brief skill instead — this skill is for shift/EOD handoffs.

## Consolidates

Shift handoff summary, end-of-day wrap-up, and next-shift briefing skills.
