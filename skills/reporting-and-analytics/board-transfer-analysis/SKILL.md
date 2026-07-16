---
name: Board Transfer Analysis
description: Someone wants to know how often tickets bounce between boards before landing — a misrouting and intake-quality signal.
category: Reporting & Analytics
tools: [search_tickets, list_boards, list_ticket_statuses]
---

# Board Transfer Analysis

Measure how often tickets move between boards before they get worked, and where the bounces cluster — a direct read on intake quality, routing rules, and where triage is failing. Frequent transfers mean delay, dropped context, and frustrated clients.

## When to use

- "How often do tickets get bounced around before someone owns them?"
- Suspicion that intake or a routing Flow is misrouting a whole category of ticket.
- Designing or auditing board structure and triage rules.

## Steps

1. Confirm the period, boards in scope (list_boards), and how a board change is detectable for this tenant — an audit/activity trail of board transitions, or a board-change count field. State the source; if no reliable transfer history exists, say so and stop rather than approximating from current board alone.
2. Run split searches to assemble the population of tickets and their transfer events per board (result-cap honesty: many small searches; record caps).
3. Report the transfer rate: share of tickets transferred at least once, average transfers per transferred ticket, and the distribution (how many bounced 2+, 3+ times).
4. **Map the flows** — the most common source→destination board pairs. A dominant lane (e.g. "Triage → Network" repeatedly) points at a fixable routing rule or an intake question that should be asked earlier.
5. Correlate transfers with delay where possible: do heavily-transferred tickets take longer to first-response or resolution? This turns "bouncing is annoying" into "bouncing costs X."
6. Output: the transfer-rate summary, the top source→destination lanes, the delay correlation, 2–3 routing/intake fixes matched to the dominant lanes, and a methodology note (transfer-detection source, boards, period, caps).

## Guardrails

- Confirm a real transfer history exists; if board changes are not tracked, do not infer misrouting from the current board — say the data is unavailable.
- Distinguish legitimate escalation transfers (tier-1 → tier-2 by design) from accidental misrouting; not every transfer is a defect. Frame the intended lanes separately.
- Attribute misrouting to rules and intake design, not to the individuals who moved the ticket.
- Never present capped searches as exact transfer counts — disclose and estimate.
- Do not invent transfer events or source/destination pairs; count ambiguous history as unclassified.
