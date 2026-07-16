---
name: Lead Monthly Ritual
description: A service manager's monthly runbook — client-risk scan, tech performance reviews, recurring-issue report, staffing glance, and automation-ROI check — run when a lead says "monthly review", "run my month-end", or "monthly management pass".
category: Role Rituals
tools: [search_tickets, search_members, list_boards]
---

# Lead Monthly Ritual

The service manager's month-end step-back: where is client risk building, how are the people trending, what keeps recurring, is staffing right, and is the automation earning its keep. A half-day, once a month.

## When to use

- "Run my month-end" / "monthly service review" / "monthly management pass."
- Before a monthly leadership or board-style meeting.
- Start of month, reviewing the month just closed.

## Steps

1. **Client-risk scan (30 min)** — run `skills/account-management/client-risk-scan`: which clients show volume spikes, sentiment decline, or repeated escalations. Hand genuine risks to the CSM lane (`skills/role-rituals/csm-weekly-ritual` owners) rather than absorbing them.
2. **Tech performance reviews (45 min)** — run `skills/reporting-and-analytics/tech-performance-review` per direct report over the month. Trends over snapshots; pair every corrective signal with the context from daily coaching notes before drawing conclusions.
3. **Recurring-issue report (30 min)** — run `skills/reporting-and-analytics/recurring-issue-report`: the issues that keep coming back. Top recurring issue becomes either a problem ticket (`skills/escalation/problem-ticket-creation`) or an automation candidate for step 5.
4. **Staffing glance (20 min)** — run `skills/reporting-and-analytics/staffing-model-analysis` lite: is volume-vs-capacity trending toward a gap in the next quarter? A glance, not a full model — escalate to the full analysis only if the glance says so.
5. **Automation ROI check (20 min)** — run `skills/reporting-and-analytics/automation-roi-report`: what the desk's automations saved this month, which underperformed, and one candidate to add or fix (route to `skills/role-rituals/automation-owner-monthly-ritual` if that role exists).
6. Output the monthly lead brief: client risks with owners, per-tech one-liners (private detail stays in review docs), the top recurring issue and its action, staffing verdict (fine / watch / act), and automation verdict.

## Time-short version

Steps 1 and 2 — client risk and people are the two things a month of delay makes materially worse. Recurring issues, staffing, and ROI slip to next month with a note.

## Guardrails

- Performance detail is for review docs and 1:1s; the shareable brief carries trends, not gradebooks.
- Risk calls need evidence (tickets, scores, dates) — a client on the risk list without citations comes off the list.
- Every finding leaves with an owner or it didn't happen: risks → CSM, recurring issue → problem ticket/automation, staffing gap → exec conversation.
- Skip-with-note beats fake completion; disclose result caps on month-scale searches.
