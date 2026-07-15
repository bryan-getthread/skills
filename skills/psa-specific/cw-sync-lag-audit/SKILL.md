---
name: CW Sync Lag Audit
description: For desks synced to ConnectWise Manage — sweep for Thread↔CW divergence (status, owner, board mismatches), separate real drift from sync lag, and propose safe reconciliation with CW as master.
category: PSA-Specific
tools: [search_tickets, list_boards, list_ticket_statuses, update_ticket, add_ticket_note]
---

# CW Sync Lag Audit

Applies to: **ConnectWise Manage**. CW→Thread sync can lag or drop individual updates, leaving tickets that are closed in CW but open in Thread, owned by different people, or sitting on different boards. This skill finds the divergence, filters out transient lag, and reconciles what remains — always toward the PSA.

## When to use

- "Do a sync audit" / "why do Thread and ConnectWise disagree?"
- Periodic hygiene sweep (weekly, or after a CW maintenance window / integration outage).
- A tech reports one mismatched ticket and you want to know how widespread it is.

## Steps

1. Scope the sweep: which boards, what time window (default: tickets touched in the last 14 days). Get the live board and status lists with `list_boards` / `list_ticket_statuses`.
2. Sweep with `search_tickets` — one search per signal per board, not one giant query. Signals: open-in-Thread tickets with no activity since a CW-side close is suspected; tickets whose owner field is empty or changed unexpectedly; tickets on a board that does not match their type. If any search may have hit a result cap, state that the count is a floor, not a total.
3. For every candidate, **re-fetch the full ticket detail individually** before calling it divergent. Most "mismatches" are sync lag, and a fresh fetch resolves them. Only a ticket that still disagrees on a fresh, full-detail read counts.
4. Classify each confirmed divergence: status drift (most common: closed in CW, open in Thread), owner drift, board drift. Note age and whether the ticket has client-visible activity pending.
5. Propose reconciliation with **CW as master**: align Thread's status/owner/board to the CW state via `update_ticket`, with a plain-text `add_ticket_note` on each recording "reconciled to PSA state on <date>, was <X> in Thread". Never push Thread's state back into CW during an audit.
6. Output a report: per-board counts, the confirmed divergence list (ticket number, field, Thread value vs CW value, age), the proposed fixes, and any tickets you are *not* confident about. Apply fixes only after the reviewer confirms — never bulk-reconcile unconfirmed.

## Guardrails

- **Sync lag is the null hypothesis.** No ticket is "divergent" until re-fetched individually at full detail. Never reconcile from list-view or cached data.
- **PSA is always master.** Reconciliation moves Thread toward CW. If someone believes CW is the wrong one, that is a manual CW-side fix for a human, not this skill.
- Never close a ticket in reconciliation that has an unanswered client message — flag it for a human instead.
- Result-cap honesty: report caps explicitly; a capped sweep is a sample, not a census.
- Notes are plain text (they sync back to CW).
- When in doubt about any single ticket, exclude it from the fix list and report it separately.

## Cross-references

- Generic pattern: `skills/psa-specific/psa-is-master-reconciliation`
- Halo equivalent: `skills/psa-specific/halo-sync-audit`
- Single-ticket status decisions: `skills/psa-specific/cw-status-workflow-mapping`
