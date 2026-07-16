---
name: Board Health Snapshot
description: Someone asks how a specific board is doing — its open, aging, unassigned, and waiting profile — or wants a cleanup score before a queue-hygiene push.
category: Reporting & Analytics
tools: [search_tickets, list_boards, list_ticket_statuses]
---

# Board Health Snapshot

Profile one board's current state — open volume, aging distribution, unassigned and waiting counts, status hygiene — and roll it into a cleanup score with the shortest path to improving it.

## When to use

- "How healthy is the <board> board right now?"
- "Give me a cleanup score before we do queue hygiene this Friday."
- A lead inheriting a board and wanting a baseline snapshot.

## Steps

1. Resolve the board (list_boards) and its status set (list_ticket_statuses), identifying which statuses mean active, waiting-on-client, scheduled/parked, and auto-closed.
2. Run split searches per signal on that board (disclose caps):
   - Total open, and open by status.
   - **Aging buckets** — e.g., 0–7d / 8–30d / 31–90d / 90d+.
   - **Unassigned** — open with no owner, split by priority.
   - **Waiting** — waiting-on-client or vendor, flagging ones with no follow-up in 14+ days.
   - **Status hygiene** — tickets sitting in intake/new statuses beyond a day, or in statuses that look misused.
3. Compute a **cleanup score** (0–100, higher = healthier) from stated weights, for example: aging burden 35%, unassigned urgency 25%, stale-waiting 20%, status hygiene 20%. Show the sub-scores, not just the total.
4. List the top 3 cleanup moves ranked by score impact ("re-drive the 12 stale waiting tickets", "own or close the 90d+ tail", "assign the 4 unassigned P2s").
5. If a prior snapshot exists in the conversation, show the delta. Close with a methodology note (statuses classified how, searches, caps).

## Guardrails

- The cleanup score is a relative hygiene heuristic — publish its formula in the output and never present it as an objective benchmark across MSPs.
- A big open count is not itself unhealthy for a high-intake board; the score weighs age, ownership, and staleness, never raw volume alone.
- Tickets properly parked on a scheduled date are healthy — do not count them as aging or stale.
- Exclude auto-closed statuses from open/aging math; if this is a junk/NOC board, say the score is not comparable to human boards.
- Recommendations only — do not close, reassign, or bulk-update anything from this snapshot.

## Unattended (Flows) variant

- Follows the Unattended Output Discipline contract: the entire reply is the plain-text snapshot — score headline with sub-scores, the per-signal table, the top 3 cleanup moves, and the methodology note (status classification, searches, caps). No narration.
- Deterministic inputs from the flow: the board, the score weights, and — for a trend line — the prior score if the flow supplies one; a scheduled run never invents last week's number.
- Board unresolvable or status set unreadable → output nothing.
- Capped searches are labeled "at least N" per signal and the headline gains `SWEEP PARTIAL`.
- Permitted writes: none. Closing, reassigning, and bulk updates stay attended — the snapshot measures, it never cleans.
