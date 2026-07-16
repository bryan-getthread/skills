---
name: Exec Weekly Ritual
description: An owner/exec's weekly 20-minute runbook — exec scorecard, resolve one decision ask, and check what's been escalated to me — run when an exec says "run my weekly", "how's the desk", or on a scheduled Monday flow.
category: Role Rituals
tools: [search_tickets, list_boards]
---

# Exec Weekly Ritual

The owner's weekly read in 20 minutes: the scorecard, one decision actually made, and nothing rotting in the "waiting on the boss" pile. Built for people who will skip anything longer.

## When to use

- "Run my exec weekly" / "how's the service desk this week" / "what needs a decision from me."
- Monday morning before the leadership meeting.
- A scheduled weekly flow posting the exec's read.

## Steps

1. **Scorecard (8 min)** — run `skills/finance-and-billing/weekly-exec-scorecard`: the week's numbers against target — revenue-relevant metrics, desk health, one trend arrow per line.
2. **One decision ask resolved (7 min)** — from the scorecard and standing asks (pricing exception, hire req, tooling spend, policy call), pick the ONE decision the team is most blocked on and resolve it now: decide, or name exactly what's missing to decide and who provides it by when. "Noted" is not a resolution.
3. **Escalations-to-me check (5 min)** — search for tickets/threads escalated to the exec or waiting on owner input (`search_tickets` on the relevant status/board). Anything older than a week gets decided, delegated with authority, or explicitly killed — the exec queue is where escalations go to die unless this step exists.
4. Output the exec card: scorecard headline (3–5 lines), the decision made and who was told, and the escalation queue disposition (count → zero, or the named survivors with checkpoint dates).

## Time-short version

Step 2 only. The scorecard can be read async; the one decision is the part that requires the exec to actually be present. Note the skips.

## Guardrails

- One decision means one, resolved — the ritual's value is throughput of decisions, not review of dashboards.
- Decisions get communicated to the person waiting on them as part of the ritual, not left in the exec's notes.
- Scorecard numbers come from the underlying skill's real queries; never let a flat week get smoothed into "on track."
- Skip-with-note beats fake completion.
- Output lands where the exec already looks (their channel/notes), and decision communications go to the waiting party's channel.

## Unattended (Flows) variant

- Your entire reply is the posted exec card, verbatim — no narration.
- Read-only: post the scorecard, the top decision ask (framed with options), and the escalation-queue list. Decisions are never made autonomously.
- Print all sections even when empty; on data failure post what succeeded plus "data incomplete".
