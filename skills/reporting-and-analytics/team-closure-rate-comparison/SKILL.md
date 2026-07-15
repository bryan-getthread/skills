---
name: Team Closure Rate Comparison
description: A lead asks to compare how many tickets each tech is closing per day, or who closed the most this week — a closure comparison that carries role and shift context.
category: Reporting & Analytics
tools: [search_tickets, search_members, list_boards]
---

# Team Closure Rate Comparison

Compare per-day closure rates across the team over a period, adjusted for role, shift, and days actually worked — so the comparison informs coaching and staffing instead of producing a misleading leaderboard.

## When to use

- "Compare closures per tech per day for the last two weeks."
- "Who is closing the most / least tickets?"
- Checking whether a shift or sub-team is keeping up with intake.

## Steps

1. Confirm the period (default: last two full weeks) and team members (search_members).
2. Run one closed-tickets search per tech per board segment where needed to stay under result caps; disclose any caps hit.
3. Exclude auto-closed statuses, junk/NOC board closures, and merged-away duplicates from every tech's credit.
4. Compute closures per working day per tech, noting visible schedule gaps (days with zero activity likely PTO or off-shift — flag, don't penalize).
5. Present the comparison **grouped by role and shift**: L1 intake techs versus escalation engineers versus dispatchers/PMs, with the expectation stated for each group. Overnight/weekend shifts get compared to their own intake, not the day shift's.
6. Call out genuine outliers within a role group only, with a plausible-cause hypothesis (heavy project week, complex escalations held, alert storm absorbed) and a suggested follow-up question for the manager.

## Guardrails

- Never rank the whole team on raw closure counts across roles — that is the exact failure this skill exists to prevent.
- Closure count alone is not a performance signal; pair outlier flags with quality context (reopens, sentiment) or mark them "count only — verify before acting."
- Do not infer PTO, sickness, or effort as fact; flag schedule-shaped gaps as hypotheses for the manager to confirm.
- Manager-facing output; do not post to tickets or shared client channels.
- End with a methodology note: period, boards, exclusions, searches, caps.
