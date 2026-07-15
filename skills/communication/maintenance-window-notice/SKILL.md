---
name: Maintenance Window Notice
description: Draft a planned-work notice for clients — what's happening, when, expected impact, duration, and the rollback promise — for patching, upgrades, migrations, or scheduled downtime.
category: Communication
tools: [search_tickets, view_openDraft]
---

# Maintenance Window Notice

Drafts the advance notice for planned work so clients are informed, not surprised — and so nobody schedules a board meeting inside your maintenance window.

## When to use

- "Draft the maintenance notice for Saturday's server patching."
- "Let the client know we're migrating their email this weekend."
- Any planned change with client-visible impact needs its heads-up email.

## Steps

1. Pull the specifics from the ticket or change record: what work, which systems, date, start time **with timezone**, expected duration, and expected impact during the window.
2. Draft per the Email Baseline Standard skill (communication/email-baseline-standard):
   - **What and when:** the work in plain terms, date, start–end time with timezone, stated twice if the window crosses midnight or a weekend.
   - **Impact:** exactly what will be unavailable, degraded, or unaffected during the window. "No impact expected" only when the record supports it.
   - **Duration honesty:** the window is the outer bound; note that service often returns sooner.
   - **Rollback promise:** if anything doesn't go to plan, work will be rolled back to the current state and rescheduled — clients are never left worse than they started.
   - **What to do:** anything the client should do before/after (save work, reboot, log out), and how to reach the desk if something looks wrong after the window.
3. State when they'll hear from us again: confirmation after completion, or an update if the window extends.
4. Present as an open draft via view_openDraft. Sending — and choosing the audience — is the technician's action.

## Guardrails

- Times must come from the scheduled record, never assumed. Always carry the timezone; multi-region clients get times in their local zone or an explicit zone label.
- Never promise "no downtime" unless the change record explicitly says so; "brief interruptions possible" is the honest default.
- The rollback promise must reflect an actual rollback plan — if the change record shows none, flag that to the technician instead of promising one.
- Fill every placeholder (<date>, <window>, <system>) before presenting, or flag unfilled ones at the top ("NEEDS: exact window").
- Degradation: view_openDraft is in-app only — over external MCP, output the notice in chat labeled "MAINTENANCE NOTICE DRAFT."
