---
name: PSA Billing Cycle Prep
description: Month-end billing readiness sweep for any PSA-synced desk — find unposted time, done-but-open tickets, and agreement anomalies before finance pulls the invoice run, so the numbers are clean at the handshake.
category: PSA-Specific
tools: [search_tickets, list_boards, list_ticket_statuses, search_members, search_clients]
---

# PSA Billing Cycle Prep

Applies to: **any PSA** (ConnectWise Manage, Autotask, HaloPSA). This is the **manual, on-demand** readiness sweep a lead or coordinator runs shortly before finance closes the billing period. The PSA runs invoicing; Thread's job here is to surface what would make the invoice run wrong — time that hasn't been posted, work that's finished but still shows open, and tickets whose agreement/billing attributes look anomalous — so they get fixed *before* finance pulls the run, not after a client disputes an invoice.

This is a finance **handshake**, not the billing analysis itself. For the hours-by-tech/client breakdown and unbilled-exposure math, use `skills/finance-and-billing/billable-analysis`; this skill finds the *readiness gaps* that would corrupt those numbers.

## When to use

- "Are we ready for month-end billing?" / "Run a billing readiness check before I hand off to finance."
- The last few days of a billing period, before the invoice run is pulled.
- After a busy period when time entries and closures may be lagging.
- Finance reports invoice discrepancies and you want to catch the same class of gaps earlier next cycle.

## Steps

1. Confirm scope with the requester: the billing period, which boards/queues are in scope, and whether all clients or a subset. If no period is given, default to the current calendar month and say so.
2. **Unposted / missing time.** Search the period's worked tickets with `search_tickets` (one search per board or per client so result caps hit per-slice, not globally) and flag tickets that show activity or closure but carry no time entries, or entries that appear unposted. Do not assert an entry is unposted if Thread cannot see posting state — report it as "no visible time / posting state unconfirmed in Thread" and point finance to the PSA.
3. **Done-but-open.** Find tickets that are effectively complete but not in a billing-ready state — resolved-but-not-closed, or sitting in a stale in-progress status past their activity. Verify status names against `list_ticket_statuses` per board (statuses are per-board). These are work that either should be closed (so it bills) or is genuinely still open (so it shouldn't) — the point is to force the decision before the run.
4. **Agreement / billing anomalies.** Surface tickets whose agreement, contract, or billing attributes look off relative to the client's norm — no agreement where one is expected, an agreement type that mismatches the work, or a client with a sudden swing in billable volume. Report these as anomalies to review, not as corrections to make.
5. **Reconcile against the PSA before trusting a gap.** Sync lag makes Thread-side state look wrong when the PSA is already right — re-fetch full detail (`search_tickets`) on any flagged ticket, and treat the PSA as master per `skills/psa-specific/psa-is-master-reconciliation`. A lag artifact is not a billing gap.
6. Output a plain-text readiness report grouped by gap type: unposted/missing time, done-but-open, agreement anomalies — each with ticket references, the client, and the specific gap. Lead with a one-line "ready / not ready" verdict and counts. Mark which counts are exact and which hit a search cap. End with the handoff line: what a human must fix **in the PSA** before the run.

## Guardrails

- **Read-only and advisory.** This skill changes nothing — it produces the list finance and the desk act on. It never posts time, never closes tickets, never edits agreements. Recommendations are not actions (zero-assumption rule).
- **The PSA is master and owns billing.** Any correction happens in the PSA by a human with access; do not "fix" Thread-side state to make the report look clean.
- **Result-cap honesty.** If a search may have hit a cap, say "at least N" — never present a capped count as exact. Split searches per board/client per signal.
- **Don't infer billability or posting state Thread can't see.** If invoicing/posting status lives only in the PSA, say so and hand it off; never label something billed/unbilled on a guess.
- Sync lag is the null hypothesis for any single flagged ticket — re-fetch before trusting divergence.
- Plain-text output; no markdown or emojis in anything destined to sync to a PSA note.
- Desks running non-English: gap categories are labels, not idioms — keep them literal.

## Cross-references

- Hours/exposure breakdown: `skills/finance-and-billing/billable-analysis`
- Reconciling flagged divergence: `skills/psa-specific/psa-is-master-reconciliation`
- Closed-status taxonomy (what "billing-ready closed" means per desk): `skills/psa-specific/psa-closed-status-taxonomy`
- Time-entry conventions (CW): `skills/psa-specific/cw-time-entry-conventions`
