---
name: Autotask Contract Categories
description: For desks synced to Autotask — read the client's Autotask contract type (recurring, block hours, retainer, T&M, fixed price) at triage, label covered vs billable, and leave burn-down notes for hour-based contracts.
category: PSA-Specific
tools: [search_tickets, search_clients, update_ticket, add_ticket_note]
---

# Autotask Contract Categories

Applies to: **Autotask (Datto/Kaseya)**. Autotask bills through contracts, and the contract type changes what a ticket costs the client: Recurring Service (covered), Block Hours / Retainer (covered until the balance exhausts), Time & Materials (billable), Fixed Price, Per Ticket. Knowing the contract at triage — especially the remaining balance on hour-based contracts — prevents surprise invoices and silent over-servicing.

## When to use

- Triage on an Autotask desk with mixed contract types: "is this covered?"
- Work on a block-hour or retainer client where burn-down matters.
- A tech is about to sink hours into a T&M client without expectations set.

## Steps

1. Re-fetch the ticket with `search_tickets` and confirm the client with `search_clients` — coverage read against the wrong company is worse than no read.
2. Determine the contract in effect. Evidence order: contract fields visible on the synced ticket/company; the desk's documented client-contract sheet; comparable recent tickets for this client. If none show it, ask. Never infer contract type from client size or tone.
3. Classify the work against the contract type:
   - **Recurring Service:** routine support covered; projects and out-of-scope items typically excluded — check the desk's exclusion list.
   - **Block Hours / Retainer:** covered while balance remains. If the desk tracks balance, state hours remaining if visible; if not visible from Thread, say so and recommend a check in Autotask before large efforts.
   - **T&M:** billable — expectations should be set with the client before significant work (`skills/communication/expectation-setting-acknowledgment`).
   - **Fixed Price / Per Ticket:** flag scope carefully; extra work erodes the fixed fee.
4. Label the ticket per desk convention with `update_ticket` and record the coverage reasoning in a plain-text `add_ticket_note`.
5. For hour-based contracts, add the **burn-down note** convention: each significant work session's note states hours consumed this ticket so the client-facing balance story stays reconstructible ("2.0h this session; ticket total 5.5h"). Do not state contract balances you cannot see.
6. Out-of-scope work routes through `skills/finance-and-billing/out-of-scope-billing-flag`; this skill labels, it does not quote or bill.

## Guardrails

- **Never guess coverage.** Low confidence → label "coverage unverified" and ask. A wrong covered/billable call is a revenue or trust incident.
- **Sync lag:** re-fetch full ticket detail before labeling; contract association and company can be corrected Autotask-side at any time.
- Burn-down figures must come from evidence (logged time on tickets you can see). Never state or estimate the contract's remaining balance unless it is actually visible; report "balance not visible from Thread" instead.
- Plain-text notes only; never put rates or amounts in notes the client can see.
- Degradation: if this tenant does not sync contract data into Thread, this skill runs in advisory mode from the desk's documented sheet only — state that limitation in every output.

## Cross-references

- CW equivalent: `skills/psa-specific/cw-agreement-aware-triage`
- Billing follow-through: `skills/finance-and-billing/billable-analysis`, `skills/finance-and-billing/out-of-scope-billing-flag`
