---
name: Triage Agent Tuning
description: Tune the Triage Agent's custom rules from observed misses — wrong boards, wrong priorities, tickets that should have been left alone — including analysis of how often techs use the bypass word. Use when asked "the triage agent keeps misrouting X", "tune our triage rules", or "review triage agent performance".
category: Automation & Flows
tools: [search_tickets, list_boards, list_ticket_priorities, list_ticket_statuses]
---

# Triage Agent Tuning

Turn a pile of "the triage agent got this wrong" examples into a small set of precise custom-rule edits, backed by evidence of how often each miss pattern actually occurs — plus a read on bypass-word usage, which is the desk telling you where it doesn't trust the agent.

## When to use

- "The triage agent keeps putting <vendor> alerts on the wrong board."
- "Techs are bypassing triage constantly — figure out why."
- Monthly review of triage accuracy after rule or board changes.

## Steps

1. Collect the misses. Start from examples the member supplies; then widen with `search_tickets` for the same pattern (same sender, subject shape, or client) over the last 30 days to measure how often it recurs. Split searches per board/signal; report capped counts as "at least N".
2. Classify each miss: wrong board, wrong priority, wrong type/classification, acted when it should have skipped, or skipped when it should have acted.
3. **Bypass-word analysis:** search recent tickets/notes for the tenant's bypass word. High usage concentrated on one board or ticket shape marks exactly where a rule is missing or wrong; broad scattered usage suggests a trust problem rather than a rule problem. Report frequency, where it clusters, and the apparent reason.
4. Draft custom-rule text for each confirmed pattern. Rules should be few, specific, and testable: name the observable signal (sender domain, subject pattern, keyword) and the exact outcome (board, priority via `list_boards`/`list_ticket_priorities` values). Prefer one rule that covers a pattern over five that cover instances.
5. For each proposed rule, show: the evidence (ticket count), the rule text, and 1–2 recent tickets it would have handled correctly — plus any ticket it might now handle *incorrectly* (over-matching check).
6. Output the rule set for the admin to paste into the Triage Agent settings, ordered by impact. Recommend re-running this review two weeks after applying.

## Guardrails

- Triage Agent rules live in settings there is no API write for — this skill produces rule text; the admin applies it. Never claim a rule has been applied.
- Every proposed rule needs evidence of recurrence; do not write rules from a single anecdote.
- Watch for over-matching: a rule that fixes 10 misses but breaks 50 correct routings is a regression — always run the over-matching check.
- Keep the total rule set small; recommend deleting stale rules when the pattern they fixed no longer appears.
