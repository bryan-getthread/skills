---
name: Disaster Recovery Plan Doc
description: Produce or refresh a per-client DR/BCP plan document — RTO/RPO targets, what's protected, recovery order, and runbook links — as a written plan, sourced from real backup and environment records. This documents the plan; it does not test it.
category: Documentation
tools: [search_clients, search_tickets, search_itglue, search_hudu, search_knowledge_base, notion-search, notion-create-pages, notion-update-page, add_ticket_note]
---

# Disaster Recovery Plan Doc

Builds the client's disaster-recovery / business-continuity plan as a document: what is protected, the RTO/RPO targets, the order systems come back, who does what, and where the step-by-step recovery runbooks live. It is a plan on paper — verifying it works is a separate exercise (a DR test), out of scope here.

## When to use

- "We need a documented DR plan for <client>" — for the client, an audit, or cyber-insurance.
- Onboarding a client and populating the backup/DR section of the doc pack (see documentation/client-onboarding-documentation-pack).
- A DR plan exists but is stale after infrastructure changes and needs refreshing.
- After a near-miss or outage exposed that recovery expectations were never written down.

## Steps

1. Establish the client with `search_clients`, then gather the inputs from records: `search_itglue` / `search_hudu` (where enabled) for backup/DR and environment docs; a Liongard read of the backup platform where the partner runs that inspector (verify it exists and last ran successfully; state dataprint age) for what is actually protected and last backup state; `search_tickets` for backup failures, restore tests, and outage history; `notion-search` (if connected) and `search_knowledge_base` for existing plans and recovery runbooks.
2. Draft the **DR plan** to this structure, populated from evidence and clearly marking business decisions the client must confirm:
   - **Scope & assumptions** — sites, systems, and data covered; what is explicitly out of scope.
   - **RTO / RPO targets** — per critical system, the recovery-time and recovery-point objectives. These are business decisions — record the client-agreed value, or flag "to be confirmed with client" rather than inventing one.
   - **Protected assets** — what is backed up, backup method/product, location(s) (on-site/off-site/cloud), retention, and last-verified backup state.
   - **Recovery order** — the sequence critical systems are restored in and their dependencies (identity → network → core servers → LOB apps, adapted to the environment).
   - **Roles & contacts** — who declares a disaster, who executes recovery, and the vendor escalation paths (link documentation/escalation-matrix-doc and documentation/vendor-contact-directory).
   - **Recovery runbooks** — links to the per-system step-by-step restore procedures; note any critical system that has no runbook as a gap.
   - **Alternate operations** — interim working arrangements during an outage (failover site, temporary cloud, manual process).
   - **Review & test cadence** — when the plan is reviewed and when a DR test is due (the test itself is separate).
3. Flag gaps as first-class output: systems with no defined RTO/RPO, protected-asset lists that don't match the known environment, critical systems with no recovery runbook, and backups whose last-verified state is old or unknown.
4. Set the **storage discipline**: the plan lives under the client's section in the desk's documentation platform, versioned and dated, reviewed on cadence and after major infrastructure change. Confirm the location convention rather than inventing one.
5. Output the plan as a paste-ready document plus the gap list. If the desk keeps DR plans in Notion and the destination is confirmed, create with `notion-create-pages` or refresh with `notion-update-page`; otherwise paste-ready blocks. Attach to the account/DR review ticket via `add_ticket_note` if requested.

## Guardrails

- **This documents the plan; it does not test or validate recovery.** Do not state that backups are recoverable or that RTO/RPO are met — record targets and last-verified state, and mark verification as a separate DR test.
- **RTO/RPO are business decisions** — never invent targets; record the client-agreed values or flag them for confirmation.
- Sourced facts only — protected assets and backup state trace to records/inspector reads/tickets; unknowns are flagged, not filled.
- Liongard/backup-inspector data requires the inspector to exist and have run successfully; state dataprint age and degrade to record/ticket sourcing when absent.
- **No credentials in the plan** — recovery credentials are vaulted and referenced by entry title only (see documentation/credential-storage-audit).
- Notes and documents are plain text per the Note Format Standard (documentation/note-format-standard) — they may sync to a PSA.
- Docs tools are search-only; writes go to Notion only when the member has connected it and confirmed the destination, otherwise paste-ready output. Degradation: without IT Glue/Hudu and backup-inspector reads, build from ticket history and requester knowledge and mark the plan's protected-asset section unverified.
