---
name: Time Entry Revenue Audit
description: When someone wants to find tickets that were worked with no time logged — revenue leakage by technician or period — and get the gaps fixed the right way.
category: Finance & Billing
tools: [search_tickets, search_members, log_time_entry, add_ticket_note]
---

# Time Entry Revenue Audit

Find tickets with clear work activity but zero (or implausibly little) logged time, quantify the likely revenue leakage by technician and period, and route the gaps back to the techs who can truthfully fill them.

## When to use

- "Which tickets got worked last week with no time entry?"
- "How much time are we failing to capture, by tech?"
- "Audit <tech>'s closed tickets for missing time."

## Steps

1. Confirm scope: period (default last full week, stated), and whether one tech, a team, or the desk. Resolve techs with `search_members`.
2. Pull worked tickets per tech per period with `search_tickets` — separate searches per tech (closed/updated in period), noting result caps.
3. For each ticket, compare activity to logged time. Leakage signals: replies or internal notes from the tech but zero time entries; a resolution note with no entry that day; long threads with a single token entry (e.g. 0.1h on a multi-day back-and-forth); status moved to resolved with no time at all.
4. Estimate the gap conservatively: count each no-time worked ticket at the desk's minimum increment (ask if unknown; default 0.25h and say so). Do NOT estimate from thread length — the point is to surface gaps, not to fabricate hours.
5. Output: per-tech table (tickets worked | tickets with no time | estimated uncaptured hours | worst examples with ticket refs), then a period total and, if the requester supplies an average rate, a leakage dollar range labeled as an estimate.
6. Remediation path: for each gap ticket, offer to post a plain-text note via `add_ticket_note` asking the assigned tech to add their actual time. Only call `log_time_entry` when the tech (or requester who did the work) explicitly confirms the real duration and description for a specific ticket — never backfill in bulk from estimates.

## Guardrails

- Time-entry evidence is the source of truth: a ticket with no entry is "uncaptured", not "unbilled revenue owed" — whether the time was billable depends on the agreement, which you have not seen.
- Never log time on someone's behalf from an estimate. Logged time is a legal/billing record; only write an entry the human doing the work confirmed, and attribute it correctly.
- Estimated leakage totals are labeled estimates with the assumption shown (minimum increment × ticket count). Result caps → "at least".
- Present per-tech findings as a capture-hygiene issue, not an accusation; some gaps are legitimate (duplicate tickets, no-work closes, covered-by-another-entry).
- Auto-generated activity (flows, agents, system notes) is not tech work — exclude it from "worked with no time" detection.
