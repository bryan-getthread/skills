---
name: Zapier Xero Billing
description: For Xero-shop MSPs — draft (never send) invoices from ticket time entries, and check a client's overdue-invoice standing before dispatching billable work. Approval-gated, draft-only. Use for "invoice the out-of-contract work on ticket <number>", "are they overdue before we do this", or ticket-time-to-Xero reconciliation.
category: Connectors
tools: [search_tickets, log_time_entry, send_approval, add_ticket_note, "zapier: Xero"]
---

# Zapier Xero Billing

The Xero twin of Zapier QuickBooks Time & Billing: keep ticket work and the ledger in agreement — assemble **draft** invoices from actual ticket time entries only after explicit approval, and give dispatch overdue-awareness so the desk doesn't sink billable hours into an account that hasn't paid the last three invoices.

## When to use

- "This project work is out of contract — draft the Xero invoice from the ticket time."
- Before dispatching billable/project work: "check <client>'s invoice standing in Xero first."
- Weekly reconciliation: "which billable ticket time never became a Xero invoice line?"

## Steps

**Overdue-awareness check (before billable dispatch):**
1. Resolve the Xero contact via `zapier: Xero "Find Contact"` matched from the client record — ambiguous or missing → stop and ask; a standing check against the wrong contact is misinformation.
2. Find the contact's invoices with overdue/unpaid status; report count, total, and oldest-overdue age with "per Xero as of now" provenance. This is decision input for the member — the skill never blocks work itself, and never mentions arrears to the client; that conversation belongs to the business owner.

**Draft invoice from ticket time (approval-gated):**
3. Pull the ticket's time entries (`search_tickets` including time entries): tech, duration, date, work notes, billable flag / agreement coverage as the desk records it. Coverage ambiguous ("is this under their agreement?") → ask; misbilling a covered client costs more than the invoice.
4. **Dedupe:** check Xero for an existing invoice/line referencing this ticket (ticket reference carried in line descriptions). Already invoiced → stop and report; double-billing is the failure this skill must never produce.
5. Assemble line items from actual time entries and materials on the ticket, at rates the member confirms — rates are never assumed. Each line description carries the ticket reference.
6. Send the approval: `send_approval` in Thread (or the Teams/Slack approval gate) to whoever owns billing, with full line-item detail and total. Approved → `zapier: Xero "Create Invoice"` as a **draft**; note the draft's number on the ticket. Not approved / timeout → nothing is created; note why.
7. Sending or authorising the invoice stays a human action in Xero — no skill path emails an invoice or moves it past draft.
8. **Reconciliation mode:** for a stated window, list billable ticket time with no matching Xero invoice line, and invoice lines referencing tickets with no billable time; output both mismatch lists with amounts, no writes without steps 4–6.

## Guardrails

- **Member-connected Zapier required** (tenant's Xero org). Degrade to producing the formatted line items and standing summary for manual entry. QuickBooks shop → the twin skill, Zapier QuickBooks Time & Billing.
- Draft, never send: money leaves drafts by human hands only.
- The approval gate on invoicing is mandatory and recorded on the ticket — amount, approver, when.
- Dedupe is checked against Xero at run time, not assumed from ticket notes.
- Financial figures in chat come only from tool results; overdue standing is stated with its as-of time and never relayed to the client by this skill.
- Task cost: each Zapier call = 2 tasks — one Find Contact + one invoice search per standing check; batch reconciliation reads.
