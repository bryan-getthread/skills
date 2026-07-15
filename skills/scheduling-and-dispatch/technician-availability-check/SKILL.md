---
name: Technician Availability Check
description: Answer "who is the next available technician?" — combine today's schedules, priority-weighted open load, and shift/PTO context into a ranked availability answer.
category: Scheduling & Dispatch
tools: [search_members, search_tickets]
---

# Technician Availability Check

A fast, honest answer to "who can take this?" — grounded in schedule entries, current load, and who is actually on shift, with the numbers shown.

## When to use

- "Who's the next available tech?" / "who can jump on this call right now?"
- "Is <tech> free this afternoon?"
- Picking a warm-transfer target for a live client call.

## Steps

1. Build the pool with `search_members`: active technicians on the relevant team/board. Exclude inactive members, anyone the desk has flagged as out/PTO, and — if this is for a specific ticket — that ticket's requester and anyone excluded by a client routing rule.
2. For each candidate, gather:
   - **Now:** are they inside a scheduled entry right now (onsite, booked session)? When does it end?
   - **Load:** open tickets, priority-weighted — someone with two Criticals is not "available" in any useful sense even with a clear calendar.
   - **Shift context:** are they on shift, near end of shift, or after-hours/on-call? Use the desk's stated hours; if shift data is unknown, say so instead of assuming.
3. Rank into three buckets: **free now** (on shift, no current block, light load), **free soon** (current block ends within the hour, or moderate load), **not today** (heavy load, out, off shift).
4. Answer the actual question first ("Next available: <tech> — free now, 2 open tickets, nothing scheduled until 3pm"), then show the ranked table with the numbers per candidate.
5. For a single-tech question ("is <tech> free?"), answer for that tech with the same evidence, plus their next free window.

## Guardrails

- Read-only: this skill answers; it does not assign or book. Offer the follow-on ("want me to assign it / schedule it?") and hand off to the assignment or scheduling skill.
- Show the evidence — never a bare name. The asker must be able to see why the answer is that tech.
- Availability without external-calendar data is partial: if the member hasn't connected a calendar source, say the view covers Thread schedules and load only.
- Effectively tied candidates → present both, let the human pick.
- Never mark someone available near or past end of shift for new work without flagging it; after-hours handoff rules beat raw availability.
- Result caps on ticket searches → load figures are "at least"; say so.
