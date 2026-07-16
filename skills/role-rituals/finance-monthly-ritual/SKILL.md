---
name: Finance Monthly Ritual
description: A finance/billing person's month-close runbook — billable analysis, seat-count true-up, license billing reconciliation, invoice-dispute queue, and unbilled-work exposure — run when finance says "month-end billing", "run the billing close", or "true-up time".
category: Role Rituals
tools: [search_tickets, search_clients]
---

# Finance Monthly Ritual

The MSP's billing close as one ordered runbook: know what was billable, true-up what's licensed and seated, reconcile what vendors charged, clear the dispute queue, and quantify what's leaking. Run before invoices go out.

## When to use

- "Run the billing close" / "month-end billing pass" / "true-up time."
- The days between month-end and invoice generation.
- After a billing complaint cluster, to audit the whole cycle.

## Steps

Order matters: steps 1–3 produce corrections that must land before invoices are cut.

1. **Billable analysis (30 min)** — run `skills/finance-and-billing/billable-analysis`: billable vs unbillable hours by agreement and tech for the month; flag agreements whose effective rate collapsed.
2. **Seat-count true-up (30 min)** — run `skills/finance-and-billing/seat-count-trueup`: per-seat agreements vs actual seats (joiners/leavers). Every delta becomes an invoice adjustment or a documented decision not to adjust.
3. **License reconciliation (30 min)** — run `skills/finance-and-billing/license-billing-reconciliation`: vendor license bills vs client license billing. Unbilled licenses and over-billed clients both get corrected — this cuts both ways.
4. **Dispute queue (20 min)** — run `skills/finance-and-billing/invoice-dispute-investigation` on every open dispute: resolve with evidence, credit, or a documented stand-firm. Disputes older than 30 days are the priority — aging disputes poison renewals.
5. **Unbilled exposure (20 min)** — run `skills/finance-and-billing/time-entry-revenue-audit` plus `skills/finance-and-billing/out-of-scope-billing-flag`: work performed that never became revenue (missing time entries, out-of-scope work absorbed silently). Output a single exposure number and its top three sources.
6. Output the close card: billable headline, true-up deltas applied, reconciliation corrections, dispute queue disposition, and the unbilled-exposure number with sources.

## Time-short version

Steps 2 and 3 — the two that change invoice amounts about to go out. Analysis, disputes, and exposure can follow the invoices; note the deferral.

## Guardrails

- Every adjustment carries evidence (dates, counts, vendor line items) — finance corrections without a paper trail create next month's disputes.
- Corrections in the client's favor get applied as readily as corrections in the MSP's favor; the ritual's credibility depends on symmetry.
- No invoice is created, adjusted, or sent by this ritual without human confirmation — it prepares the changes, a person approves them.
- Rate and margin data stay in finance-appropriate channels.
- Skip-with-note beats fake completion; result caps on month-scale ticket/time searches are disclosed next to any number they could distort.
