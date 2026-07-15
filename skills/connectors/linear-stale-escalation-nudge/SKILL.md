---
name: Linear Stale Escalation Nudge
description: Find field-escalation issues in Linear untouched for 14+ days and comment a nudge carrying current client impact — tickets waiting, clients affected, aging. Use for "nudge our stale escalations", "what's engineering sitting on", or a recurring escalation-hygiene sweep.
category: Connectors
tools: [list_issues, get_issue, list_comments, create_comment, search_tickets]
---

# Linear Stale Escalation Nudge

Escalations go quiet because engineering can't feel the client-side clock. This sweep finds field escalations with no activity in 14+ days and posts one respectful comment per issue with the current, quantified impact — the information that re-ranks work without a meeting.

## When to use

- "Sweep our escalations for anything engineering hasn't touched in two weeks."
- Scheduled weekly escalation-hygiene run.
- "Bug <issue id> has been silent for a month — nudge it with the damage."

## Steps

1. `list_issues` for open issues under the field-escalation label/project (confirm the tenant's marker on first use). For each, determine last activity from the issue's updated time and `list_comments` — a recent comment counts as activity even if the state didn't move.
2. Filter to issues stale past the threshold (default 14 days).
3. **Anti-nag check:** if the most recent comment is already a nudge from this process within the last 14 days, skip the issue — repeated identical nudges get the whole process ignored.
4. For each remaining issue, gather *current* impact from the ticket side: `search_tickets` for tickets referencing the issue id / matching its symptom — distinct tickets, distinct clients, oldest waiting ticket, anything new since the issue last had activity ("3 more tickets since your last update"). Capped counts reported as "at least N".
5. Present the nudge batch (issue, days stale, impact line, draft comment) for confirmation.
6. `create_comment` on each confirmed issue: neutral, factual, and useful — current impact numbers, what's changed since last activity, and a concrete ask ("status update or a revised ETA we can give the affected clients"). No names of clients unless the member opts in; no scolding.
7. Report: nudged issues, skipped (recently nudged / actually active), and any issue whose waiting tickets have all closed — flag those as candidates to close or downgrade rather than nudge.

## Guardrails

- **Member-authenticated connector:** requires the member's Linear connection; degrade by producing the stale list + draft comments for manual posting.
- Comment-only: this skill never changes issue state, priority, or assignee — nudges inform; humans re-rank (or invoke Linear Client Impact Queue deliberately).
- The 14-day anti-nag dedupe is mandatory, checked per issue against actual comments, not memory.
- Impact numbers must be current-run data, never reused from a previous sweep.
- Tone is collegial: the comment should read as helpful context from the field, not an SLA violation notice.
- Confirm before posting; cap a single run (default 15 nudges) and say when truncated.
