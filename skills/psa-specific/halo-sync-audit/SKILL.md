---
name: Halo Sync Audit
description: For desks synced to HaloPSA — sweep for Thread↔Halo divergence, with special attention to the known pattern of statuses not carrying over; reconcile toward Halo as master.
category: PSA-Specific
tools: [search_tickets, list_boards, list_ticket_statuses, update_ticket, add_ticket_note]
---

# Halo Sync Audit

Applies to: **HaloPSA**. Halo's action-driven status model means a Halo-side action can change status, visibility, and assignment in one move — and the status leg of that change is the piece most often observed not carrying over into Thread. This skill sweeps for divergence, filters out transient lag, and reconciles the rest toward the PSA.

## When to use

- "Thread says open, Halo says resolved" — one report, and you want the blast radius.
- Periodic Thread↔Halo hygiene sweep, or after an integration outage / Halo upgrade.
- Before trusting Thread-side reports that depend on Halo-synced status.

## Steps

1. Scope: which teams/boards, what window (default: tickets touched in the last 14 days). Pull live lists with `list_boards` / `list_ticket_statuses`.
2. Sweep with `search_tickets`, one search per signal per team: (a) the signature pattern — tickets whose last activity is a resolution-shaped note but whose Thread status is still open-family; (b) tickets on hold-type statuses with no stated wait reason (suggesting a Halo action synced partially); (c) owner/team mismatches; (d) tickets stale in Thread but with recent Halo-originated notes. Disclose result caps — capped sweeps are samples.
3. For each candidate, **re-fetch the full ticket detail individually** before calling it divergent. Sync lag is the null hypothesis; only tickets that still disagree on a fresh full-detail read count.
4. Classify confirmed divergences: status drift (resolved/closed in Halo, open in Thread — the known pattern), assignment drift, team drift. Note age and whether client-visible activity is pending.
5. Propose reconciliation with **Halo as master**: align Thread via `update_ticket`, each with a plain-text `add_ticket_note`: "reconciled to PSA state on <date>; Thread showed <X>". Never push Thread state back into Halo during an audit, and never reconcile a ticket carrying an unanswered client message — flag those for a human.
6. Output the report: per-team counts, confirmed divergence list (ticket, field, Thread vs Halo value, age), proposed fixes, excluded/uncertain tickets listed separately. Apply only after the reviewer confirms; if the same pattern recurs sweep after sweep, recommend the integration mapping itself be reviewed (statuses missing from the mapping table are the usual root cause — see `skills/psa-specific/psa-field-mapping-doc`).

## Guardrails

- **Sync lag is the null hypothesis** — no divergence call without an individual fresh re-fetch at full detail.
- **PSA is always master.** Thread moves toward Halo; a believed Halo-side error is a manual human fix, not a reconciliation target.
- Never bulk-apply; the confirmed list is a proposal until a human approves it.
- Result-cap honesty in every count you report.
- Plain-text notes only.
- When in doubt on any ticket, exclude and report separately — a wrong reconciliation is worse than a listed uncertainty.

## Cross-references

- Generic pattern: `skills/psa-specific/psa-is-master-reconciliation`
- CW equivalent: `skills/psa-specific/cw-sync-lag-audit`
- Why statuses drift here: `skills/psa-specific/halo-status-actions`
