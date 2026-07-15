---
name: PSA Field Mapping Doc
description: For any PSA-synced desk (ConnectWise, Autotask, HaloPSA) — build and maintain the desk's Thread↔PSA field-mapping cheat sheet (statuses, boards/queues, priorities, classification values) from observed tickets, not assumptions.
category: PSA-Specific
tools: [search_tickets, list_boards, list_ticket_statuses, list_ticket_priorities, search_knowledge_base, add_ticket_note]
---

# PSA Field Mapping Doc

Applies to: **any PSA** (ConnectWise Manage, Autotask, HaloPSA). Nearly every PSA-specific skill needs the same artifact: a trustworthy map of how Thread's boards, statuses, priorities, and classification values correspond to the PSA's. This skill builds that cheat sheet empirically — from the tenant's live lists and observed tickets — and keeps it current.

## When to use

- Another PSA skill hit an unmapped value ("what does 'Pending Review' correspond to in the PSA?").
- Onboarding a new desk or after PSA config changes (new board, renamed statuses).
- A sync audit keeps finding the same divergence and the mapping table is the suspected root cause.

## Steps

1. Pull the tenant's live vocabulary: `list_boards`, `list_ticket_statuses` (per board where board-scoped), `list_ticket_priorities`. These lists are the Thread-side ground truth — a mapping doc containing values these calls do not return is fiction.
2. Enrich per-board with observed usage: sample recent tickets per board via `search_tickets` (disclose caps) and record which classification values (CW Type/Subtype/Item, Autotask Issue/Sub-Issue, Halo categories) actually appear, and which statuses tickets really flow through vs merely exist.
3. Structure the cheat sheet per section, marking each row's evidence level (**live-list** / **observed** / **desk-stated** / **unverified**):
   - Boards ↔ PSA boards/queues/teams, and what work belongs on each.
   - Statuses per board ↔ PSA statuses, flagged: closed-family? clock-pausing? notification-firing?
   - Priorities ↔ PSA priorities and any SLA linkage.
   - Classification values observed, with valid parent-child pairings.
   - Known sync quirks for this tenant (fields that lag, values that don't carry — feed from `skills/psa-specific/cw-sync-lag-audit` / `skills/psa-specific/halo-sync-audit` findings).
4. Cross-check with the desk: anything only inferable ("does 'Scheduled' pause your SLA clock?") goes in as a question, not an answer. Never promote an inference to fact without confirmation.
5. Publish where the desk keeps standards — `search_knowledge_base` to find an existing doc to update rather than fork; keep exactly one living copy. If the desk stores docs in a connected tool (e.g. Notion via `skills/connectors/notion-sop-publishing`), route there.
6. Maintain: on each rerun, diff live lists against the doc, flag added/renamed/removed values, and mark stale rows rather than silently deleting them.

## Guardrails

- **Evidence-labeled rows only.** Every mapping row carries its evidence level; "unverified" rows exist to be visible, not to be trusted.
- **Sync lag:** when sampling tickets to observe values, re-fetch full detail on anything you cite as an example — a stale sample poisons the doc.
- No tenant identifiers beyond what the desk's own doc needs; never paste client names or people into example rows — use placeholders.
- Result-cap honesty: observed-value coverage from capped samples is partial; say so in the doc header.
- One living document. Do not generate a fresh doc per run; update in place and date the revision.
- This skill documents the mapping — it never changes tickets to match the doc. Reconciliation belongs to the audit skills.

## Cross-references

- Consumers: `skills/psa-specific/cw-type-subtype-item`, `skills/psa-specific/autotask-ticket-categories`, `skills/psa-specific/halo-status-actions`, `skills/psa-specific/psa-closed-status-taxonomy`
- Publishing: `skills/documentation/sop-builder`, `skills/connectors/notion-sop-publishing`
