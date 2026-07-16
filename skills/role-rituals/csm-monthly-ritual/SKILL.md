---
name: CSM Monthly Ritual
description: A CSM's month-end account runbook — client health-report cycle, health-score reconciliation, and renewal prep for upcoming renewals — run when a CSM says "run my month-end", "monthly account review", or "health report cycle".
category: Role Rituals
tools: [search_tickets, search_clients]
---

# CSM Monthly Ritual

The CSM's month-end sweep: produce the health reads, make sure the scores match reality, and get ahead of every renewal inside the horizon. A half-day at month boundary.

## When to use

- "Run my CSM month-end" / "monthly health cycle" / "which renewals are coming."
- First week of the month, covering the month just closed.
- Before a CS team review where account health is reported up.

## Steps

1. **Health-report cycle (60+ min)** — run `skills/account-management/client-health-report` for each account due this month (all accounts monthly, or the tiered cadence the CSM uses — top tier monthly, others quarterly). Batch by tier; do the at-risk accounts first while attention is freshest.
2. **Health-score reconciliation (30 min)** — run `skills/account-management/health-score-reconciliation`: accounts whose recorded health score disagrees with what step 1's evidence says. Correct the score or open a question — a wrong score quietly misroutes everyone else's attention.
3. **Renewal prep (30 min)** — list agreements renewing within the prep horizon (typically 90 days) and run `skills/account-management/renewal-prep` for each. Any renewal inside 60 days without a prepared story is this month's top priority.
4. Output the monthly account card: health reports produced (per account, one-line verdict), score corrections made, and the renewal runway table (client, date, prep status, open risk).

## Time-short version

Step 3 (renewals — dates don't move) plus step 1 restricted to at-risk accounts only. Full-cycle reports and reconciliation slip with a note.

## Guardrails

- Health verdicts come from evidence gathered by the underlying skills, never from last month's verdict copied forward.
- Score changes are recorded with a one-line reason; silent score edits erode trust in the whole system.
- Renewal prep is internal until the CSM chooses to engage — nothing renewal-related is sent to clients from this ritual.
- Tiered cadence is fine; fake completeness is not — report "12 of 30 accounts reviewed (tier 1)" honestly.
- Skip-with-note beats fake completion; disclose result caps on portfolio-wide pulls.
