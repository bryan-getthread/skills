---
name: Lead Weekly Ritual
description: A service manager's weekly runbook — ops report, QA digest, 1:1 prep for the week's scheduled sit-downs, wins roundup, and one process fix picked — run when a lead says "run my weekly", "prep my week", or "weekly management pass".
category: Role Rituals
tools: [search_tickets, search_members, list_boards]
---

# Lead Weekly Ritual

The service manager's weekly operating rhythm in one runbook: read the week, check quality, prep the people conversations, celebrate what worked, and commit to fixing one thing. ~60–90 minutes, same day each week.

## When to use

- "Run my weekly" / "prep my management week" / "weekly ops pass."
- Monday planning or Friday close-out, per the lead's rhythm.
- Before a leadership meeting where the lead reports on the desk.

## Steps

1. **Ops report (20 min)** — run `skills/reporting-and-analytics/weekly-ops-report`: volume, closure, SLA, backlog movement, staffing signals for the week.
2. **QA digest (15 min)** — run `skills/qa-and-closure/weekly-qa-digest`: closure quality, reopen patterns, note completeness. Carry any per-tech quality signal into step 3, not into public output.
3. **1:1 prep (15 min)** — for each direct report with a 1:1 scheduled this week, run `skills/reporting-and-analytics/one-on-one-prep`, folding in the daily coaching observations captured by `skills/role-rituals/lead-daily-ritual`.
4. **Wins roundup (10 min)** — run `skills/reporting-and-analytics/weekly-wins-roundup` and post it where the team sees it. Recognition is part of the operating rhythm, not an extra.
5. **One process fix (10 min)** — from steps 1–2, pick ONE recurring friction (a routing miss, a closure-note pattern, an SLA leak) and open a concrete action: a ticket, a flow change proposal (via `skills/automation-and-flows/flow-builder` if automation-shaped), or an agenda item with an owner and a date.
6. Output the weekly lead card: week's headline numbers, QA themes, 1:1s prepped (names only), wins posted, and the one process fix with its owner.

## Time-short version

Steps 3 and 5 — people conversations and the one fix are what compound; reports can be read late, and the wins roundup can move to the huddle. Note the skips.

## Guardrails

- One process fix means one, with an owner and a date — a list of ten fixes is a wish, not a ritual outcome.
- Per-tech QA findings route to 1:1s, never to team-visible output.
- Wins must cite real tickets/events from this week; skip the roundup rather than pad it.
- Skip-with-note beats fake completion; disclose result caps on weekly searches.
- Outputs land where the lead already looks: reports to their notes/channel, 1:1 preps to their 1:1 docs, wins to the team channel.
