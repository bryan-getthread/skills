---
name: CW Agreement-Aware Triage
description: For desks synced to ConnectWise Manage — read the client's CW agreement type at ticket intake and route/label the work as covered vs billable before anyone touches it.
category: PSA-Specific
tools: [search_tickets, search_clients, update_ticket, add_ticket_note, list_boards]
---

# CW Agreement-Aware Triage

Applies to: **ConnectWise Manage**. Every CW client has zero or more agreements (managed services, block time, T&M, project) that decide whether work is covered or billed. Reading the agreement at intake — not at invoicing — prevents scope disputes and mis-billed time.

## When to use

- Triaging a new ticket on a CW-synced desk where the client mix includes different agreement types.
- "Is this covered or billable?" before starting work.
- A dispatcher wants covered/billable labeling as part of the intake pass.

## Steps

1. Re-fetch the ticket with `search_tickets` (full detail) and identify the client with `search_clients`. Confirm the ticket's company matches the requester before reading any agreement — a catchall-misfiled ticket inherits the wrong agreement.
2. Determine the client's agreement context. Use, in order: agreement fields visible on the synced ticket/company record; the desk's documented client-agreement sheet; comparable recent tickets for the same client. If none of these show it, ask — never assume coverage.
3. Classify the requested work against the agreement type:
   - Managed/AYCE-type: routine support covered; projects, adds/moves/changes, and out-of-hours may not be.
   - Block time / retainer: covered until hours exhaust; note burn-down visibility if the desk tracks it.
   - T&M: billable by default; client expectations should be set early.
4. Label the ticket per desk convention with `update_ticket` (board, type, or the desk's covered/billable marker) and record the reasoning in a plain-text `add_ticket_note`.
5. If the work looks out of scope, do not refuse or quote — route through `skills/finance-and-billing/out-of-scope-billing-flag` and, for client comms, `skills/communication/scope-pushback`.
6. Output: client, agreement basis (with the evidence used), covered/billable call, confidence, and the proposed label — apply only after confirmation when confidence is not high.

## Guardrails

- **Never guess coverage.** A wrong "covered" call gives away revenue; a wrong "billable" call creates a client dispute. Low confidence → label as "coverage unverified" and ask.
- **Sync lag:** re-fetch full ticket detail before labeling; company, board, or type may have been corrected in CW since intake.
- Agreement determinations are advisory — the skill labels and routes, it does not commit billing. Final billing lives in the time entry (`skills/psa-specific/cw-time-entry-conventions`).
- Notes syncing to CW: plain text, and never state billing amounts or rates in a note the client can see.
- If this tenant does not sync agreement data into Thread, say so plainly and fall back to the desk's documented sheet rather than inferring from ticket history alone.

## Cross-references

- Billing follow-through: `skills/finance-and-billing/out-of-scope-billing-flag`, `skills/finance-and-billing/billable-analysis`
- Autotask equivalent: `skills/psa-specific/autotask-contract-categories`
