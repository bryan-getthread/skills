---
name: PSA-Is-Master Reconciliation
description: For any PSA-synced desk (ConnectWise, Autotask, HaloPSA) — the generic pattern for resolving a Thread↔PSA disagreement on a ticket: rule out sync lag first, then move Thread to match the PSA, never the reverse.
category: PSA-Specific
tools: [search_tickets, list_ticket_statuses, list_boards, update_ticket, add_ticket_note]
---

# PSA-Is-Master Reconciliation

Applies to: **any PSA** (ConnectWise Manage, Autotask, HaloPSA). When Thread and the partner's PSA disagree about a ticket — status, owner, board/queue, priority — there is one safe default: **the PSA is master**. The PSA runs billing, SLAs, and client reporting; Thread mirrors it. This skill is the single-ticket reconciliation pattern that the per-PSA audit skills apply in bulk.

## When to use

- One ticket shows conflicting state between Thread and the PSA and someone asks which is right.
- A skill or flow is about to act on a Thread-side status that smells stale.
- Any PSA-specific skill hits a disagreement and needs the standard resolution.

## Steps

1. **Rule out sync lag first.** Re-fetch the full ticket detail with `search_tickets` — not a list view, not memory from earlier in the conversation. Most disagreements dissolve on a fresh read. If it dissolves, stop: there was no divergence, and nothing gets written.
2. If the disagreement persists, characterize it precisely: which field (status / owner / board / priority), Thread value vs PSA value (as far as the PSA value is visible or reported by the requester), and when each side last changed if timestamps show it.
3. Determine the master. Default: the PSA. The only exceptions are ones the desk has explicitly documented (e.g. Thread-master during a migration phase — `skills/psa-specific/psa-migration-hygiene`). If someone asserts "Thread is right this time," that does not change the master — it means the fix belongs *in the PSA*, made by a human with PSA access.
4. Check for anything the reconciliation would trample: an unanswered client message, pending time entries, an active SLA window. Any of these → flag for a human instead of reconciling.
5. Reconcile Thread to the PSA value with `update_ticket` (validating target values via `list_ticket_statuses` / `list_boards`), and record it with a plain-text `add_ticket_note`: "Reconciled to PSA state on <date>: <field> was <Thread value>, set to <PSA value>."
6. Output: lag-ruled-out confirmation, the divergence found, the master determination, what was changed (or why it was flagged instead). If the same field keeps diverging across tickets, recommend the mapping be reviewed (`skills/psa-specific/psa-field-mapping-doc`) rather than reconciling forever.

## Guardrails

- **Sync lag is the null hypothesis** — no reconciliation without the fresh full-detail re-fetch in step 1. Reconciling a lag artifact *creates* divergence.
- **Never write toward the PSA to resolve a conflict.** Thread moves; the PSA does not. PSA-side corrections are human work.
- Never reconcile over an unanswered client message or into a closed-family status without explicit confirmation.
- One ticket at a time. Bulk divergence goes to the audit skills, which propose before applying.
- Record every reconciliation in a note — silent state changes are how the next disagreement becomes unexplainable.
- When in doubt — ambiguous master, invisible PSA value, contested history — do nothing and report.

## Cross-references

- Bulk sweeps: `skills/psa-specific/cw-sync-lag-audit`, `skills/psa-specific/halo-sync-audit`
- Migration-phase master rules: `skills/psa-specific/psa-migration-hygiene`
