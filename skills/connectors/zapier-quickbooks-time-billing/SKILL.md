---
name: Zapier QuickBooks Time & Billing
description: Push ticket time entries into QuickBooks as Time Activities and draft (never send) an invoice for out-of-contract work, gated by approval. Use for "get this ticket's time into QuickBooks", "bill the out-of-scope work on ticket <number>", or a periodic time-to-billing reconciliation pass.
category: Connectors
tools: [search_tickets, log_time_entry, send_approval, add_ticket_note, "zapier: QuickBooks Online"]
---

# Zapier QuickBooks Time & Billing

Stop revenue leaking between the ticket and the ledger: mirror ticket time entries to `zapier: QuickBooks Online "Create Time Activity"`, and when work falls outside the client's agreement, assemble a **draft** invoice — created only after an explicit approval, sent only by a human.

## When to use

- "Push the time from ticket <number> into QuickBooks."
- "This project work is out of contract — draft the invoice."
- Weekly reconciliation: "which billable ticket time never made it into QuickBooks?"

## Steps

1. Pull the ticket's time entries (`search_tickets` including time entries): tech, duration, date, work notes, and the billable flag / agreement coverage as the desk records it.
2. Resolve the QuickBooks customer: `zapier: QuickBooks Online "Find Customer"` matched from the client record — ambiguous or missing customer → stop and ask; never bill a best-guess customer.
3. **Dedupe before writing:** check for an existing Time Activity for the same tech/date/ticket (search by the ticket reference carried in the activity description). Already mirrored → skip; report it. Double-billed time is the failure this skill must never produce.
4. For each unmirrored billable entry, show the batch (tech, duration, date, description including ticket reference), confirm, then `zapier: QuickBooks Online "Create Time Activity"` per entry. `add_ticket_note` (plain text) recording what was mirrored.
5. **Out-of-contract invoice path (approval-gated):**
   - Assemble the line items from actual time entries and any materials on the ticket, at the rates the member confirms — rates are never assumed.
   - Send the approval: `send_approval` in Thread (or the Zapier Teams Approval Gate) to the member/manager who owns billing, with the full line-item detail.
   - Approved → `zapier: QuickBooks Online "Create Invoice"` as a **draft**; note the draft's existence and number on the ticket. Not approved/timeout → nothing is created; note why.
   - Sending the invoice to the client stays a human action in QuickBooks — never call the send action from this skill.
6. **Reconciliation mode:** for a stated window, list billable ticket time with no matching Time Activity, and Time Activities referencing tickets with no billable time — output both mismatch lists with amounts; make no writes without going through steps 3–4.

## Guardrails

- **Member-connected Zapier required** (tenant's QuickBooks Online). Degrade to producing the formatted time/invoice data for manual entry.
- Draft, never send: no skill path emails an invoice or marks it sent. Money leaves drafts by human hands.
- The approval gate on out-of-contract invoicing is mandatory and recorded on the ticket — amount, approver, when.
- Dedupe is checked against QuickBooks at run time, not assumed from ticket notes.
- Rates, tax treatment, and agreement coverage come from the member/tenant records; when coverage is ambiguous ("is this under their AYCE?"), ask — misbilling a covered client costs more than the invoice.
- Financial figures in chat come only from tool results; no estimated totals presented as ledger fact.
- Task cost: each Zapier call = 2 tasks — batch reconciliation reads, and mirror time in per-entry writes only after one batched confirmation.
