---
name: CW Project Tickets
description: For desks synced to ConnectWise Manage — recognize when a ticket is really a project ticket, understand the project→phase→ticket structure, and act within what Thread can actually see of the Projects module.
category: PSA-Specific
tools: [search_tickets, list_boards, list_ticket_statuses, update_ticket, add_ticket_note]
---

# CW Project Tickets

Applies to: **ConnectWise Manage**. CW splits work between the Service module (service boards) and the Projects module (projects → phases → project tickets). Project tickets look like tickets but behave differently: they live on **project boards** with their own status workflows, they carry project/phase membership, their time rolls up to project budgets, and service-desk conventions (SLA clocks, triage, closure QA) mostly don't apply. Thread's visibility into the Projects module varies by tenant — some sync project tickets like any other ticket, some don't sync them at all. This skill is the recognition-and-discipline layer.

## When to use

- A ticket on a CW-synced desk smells like project work (install, migration, rollout, multi-visit onsite) and you need to decide how to treat it.
- Someone references a ticket number that `search_tickets` can't find or that behaves oddly (statuses that don't match the service board).
- Service-desk automations or sweeps are about to touch tickets that belong to a project board.
- A tech asks whether to log time to the service ticket or the project.

## Steps

1. Re-fetch the ticket with `search_tickets` at full detail. Check its board against `list_boards`: if the board is one the desk designates as a **project board**, service-desk rules stop applying — say so before doing anything else. If the desk's board list doesn't distinguish, use the desk's documented board map; if none exists, ask rather than classify by ticket title.
2. **Recognition heuristics** (evidence, not proof — always confirm): work quoted or sold rather than reported; a phase/project reference in the ticket fields or notes; planned multi-visit scheduling; budget-hours language. A service ticket that *should* be a project (scope creep past the desk's project threshold) is a recommendation to management, not a conversion you perform — CW-side project creation is human work.
3. **Status discipline:** project boards have their own status workflows in CW. Pull `list_ticket_statuses` for that board specifically — never reuse service-board statuses. Closed-family on a project ticket can mark a phase deliverable complete; treat it as deliberate, and confirm before any closed-family move.
4. **What Thread sees:** state visibility honestly. Phase structure, project budget, remaining budget hours, and the project's overall status are generally **not visible from Thread** even when project tickets sync. Never state budget burn or phase progress you cannot see; report "project-level data not visible from Thread — check the CW Projects module."
5. **Time entries:** time on project tickets burns project budget. If a tech asks where to log, the answer follows the desk's convention (usually: log where the work order lives) — but flag that misrouted time distorts both service metrics and project profitability, and route corrections to `skills/psa-specific/cw-time-entry-conventions`.
6. **Sweep protection:** when running bulk audits or closure sweeps on a CW desk, exclude project boards unless explicitly asked to include them, and say you excluded them. Auto-closing a project ticket because it "looks stale" between phases is a classic false positive — project tickets legitimately idle between scheduled visits.
7. **Drift:** Thread↔CW disagreement on a project ticket follows `skills/psa-specific/psa-is-master-reconciliation` — CW is master; Thread moves.
8. Output: the classification (service vs project, with evidence), which conventions therefore apply, any action proposed with side effects, and an explicit list of what was not visible from Thread. Record actions with a plain-text `add_ticket_note`.

## Guardrails

- **Sync lag:** re-fetch full ticket detail immediately before trusting or changing anything; project tickets get updated CW-side during onsite visits without Thread activity.
- **PSA is always master.** Never reopen or restructure project tickets from Thread to match a Thread-side impression.
- Never invent project/phase names, budget figures, or board designations — per-tenant values come from `list_boards` / `list_ticket_statuses` or the desk's documented map only.
- Never convert a service ticket into a project (or vice versa) — that is CW-side human work; your output is a recommendation.
- Exclude project boards from service-desk bulk operations by default; when a sweep may have hit a result cap, report counts as "at least N."
- Degradation: if this tenant does not sync project tickets into Thread at all, this skill runs in advisory mode — identify the work as likely project-side and hand off; state that limitation.
- Notes syncing to CW must be plain text — no markdown, no emojis.

## Cross-references

- Board/status mechanics: `skills/psa-specific/cw-status-workflow-mapping`, `skills/psa-specific/cw-service-board-conventions`
- Time discipline: `skills/psa-specific/cw-time-entry-conventions`
- Project economics: `skills/finance-and-billing/project-profitability`
