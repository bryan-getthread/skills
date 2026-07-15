---
name: Liongard Veeam Posture Read
description: Interrogate a client's Veeam deployment through Liongard — job inventory and schedules, repository capacity, protected-VM census, license state. Use for "what does Veeam protect at <client>", coverage-gap checks, and repo-capacity questions; for a job FAILURE alert use vendor-runbooks/veeam-job-failures instead.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Veeam Posture Read

Reads Veeam Backup & Replication configuration and state — jobs, repositories, protected workloads, licensing — from the Liongard Veeam inspector. This is the **posture/state read**: what is protected, where it lands, and whether the design still covers the estate. The **alert-response** side (a job failed, work the failure) lives in vendor-runbooks/veeam-job-failures — hand off there when the trigger is a failure notification, and pull posture context from here into that investigation.

## When to use

- "What does Veeam actually protect at <client>?" — protected-VM census vs the live estate.
- "How full are their backup repositories?" — capacity before it becomes a failed-job cascade.
- "What jobs exist and when do they run?" — schedule/inventory for planning or an audit.
- Coverage-gap check: VMs in the hypervisor inventory that no job protects.
- License/support state on the Veeam server.

## What the inspector captures

Backup server identity and version, job inventory (name, type, schedule, included workloads, last-result where present), repository list with capacity/free space, protected-workload census, and license information. Coverage varies by Veeam edition and inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve environment, `liongard_launchpoint` with systemType "Veeam", verify last-run success, note dataprint age.
2. Query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Jobs: name, type, schedule, enabled state — disabled jobs are a headline finding; jobs with no recent successful result feed the failure runbook.
   - Repositories: name, capacity, free — flag <20% free; a repo trending full predicts the next week's failure tickets.
   - Protected census: workloads included across jobs, deduplicated.
   - License: edition, instances used vs licensed, support expiry — flag near-limit and near-expiry.
3. **Coverage-gap cross-check (the high-value move)**: pull the VM inventory from liongard-inspectors/liongard-vmware or liongard-inspectors/liongard-hyperv for the same environment and diff against the protected census. VMs present in the hypervisor but absent from every job = unprotected workloads — the finding partners care most about. State both dataprint ages; a gap between differently-aged dataprints gets a "verify" qualifier.
4. "What changed": `liongard_detection` / `liongard_timeline` — job adds/edits/disables, repository changes, license transitions. A job silently disabled last month is a better catch than any dashboard.
5. Output: job table, repo capacity table, protected census + coverage gaps, license line, flags with severity, source + dataprint ages. Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: restore requests and backup questions open with the job/repo facts attached.
- **Alert handoff**: when a failure alert is live, vendor-runbooks/veeam-job-failures owns the response — this skill supplies the "what changed / how full is the repo / was the job edited" context.
- **QBR / compliance evidence**: protected-vs-total counts, repo headroom, and coverage gaps are backup-posture lines for QBRs and compliance reviews (cross-ref compliance-and-audit).

## Guardrails

- Read-only: never enable/disable jobs, edit schedules, or touch repositories.
- Never present the posture read as proof backups are *working* — last-result fields in a dataprint are as-of the last inspector run; restore tests own "working". Say so when the question is really "are we safe".
- Coverage-gap findings must name the comparison basis (which hypervisor dataprint, what age) — a stale VM inventory produces false gaps.
- Deliberately-excluded VMs exist (templates, test boxes) — phrase gaps as "not found in any job — confirm intentional".
- Degrade per the access pattern (docs → ticket history) when the inspector is absent; also check devices-and-infrastructure/backup-failure-triage for the generic backup path.
- Plain-text notes only.
