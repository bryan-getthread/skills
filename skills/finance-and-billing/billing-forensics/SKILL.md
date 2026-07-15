---
name: Billing Forensics
description: When someone asks "why is the client billed X" and the charge needs to be traced to its source across tickets, time entries, agreements, and vendor invoices.
category: Finance & Billing
tools: [search_tickets, search_clients, add_ticket_note, zapier]
---

# Billing Forensics

Trace a specific charge back to its origin — the ticket, time entries, agreement line, or vendor pass-through that produced it — and present the chain of evidence, including any link that cannot be verified.

Optional Zapier reads: `zapier: QuickBooks "Find Invoice"`, `zapier: Xero "Find Invoice"` (when the member has the connector).

## When to use

- "Why was <client> billed $X in <month>?"
- "Where did this line item come from? I can't place it."
- "The account manager needs the story behind this charge before the renewal call."

## Steps

1. Pin down the charge: client, amount, invoice date/period, and line description as it appears. If the requester can paste the invoice line(s), start from that text.
2. If the member's Zapier connector includes the accounting system, pull the invoice detail with `zapier: QuickBooks "Find Invoice"` / `zapier: Xero "Find Invoice"` to get exact line items, quantities, and service dates. Otherwise work from the pasted detail.
3. Classify each line and trace it:
   - **Labor** → find the tickets and time entries behind it via `search_tickets` for the client + service dates; reconcile hours × rate to the line amount.
   - **Recurring/agreement** → match against the agreement quantity basis (seats, endpoints); quantities come from the requester's export or the agreement — cite which.
   - **Product/pass-through** → match to a procurement ticket or vendor invoice reference; if neither exists in reach, mark the link unverified.
4. Build the evidence chain per line: invoice line → source record(s) → supporting detail, with each hop explicitly cited (ticket number, entry date/tech/hours, export row). Reconcile the math: do the sources sum to the billed amount? Report any residual.
5. Deliver the story: a per-line trace table, the reconciliation result (fully explained / partially explained with residual / unexplained), and — where a line is unexplained or over-supported — what record a human should pull next (PSA agreement, vendor invoice PDF).
6. Offer to save the trace as a plain-text internal note via `add_ticket_note` on the related ticket for the account team.

## Guardrails

- Never state a contractual or billing conclusion as fact without citing the evidence for that hop. An unverifiable link is reported as "unverified", not bridged with an assumption — an honest gap beats a tidy story.
- Time-entry evidence is the source of truth for labor charges: if the billed hours exceed logged hours, report the excess as unexplained; do not rationalize it.
- Read-only forensics: no invoice edits, no credits, no client communication. What happens about a wrong charge is a human decision.
- Result-cap honesty on ticket searches — a capped search means the labor trace may be incomplete; say so before calling a charge unsupported.
- Keep the output internal; it may expose rates and margins that never belong in a client-facing surface.
- Degradation: no accounting connector → the trace quality depends on what the requester pastes; state which lines could not be examined at source.
