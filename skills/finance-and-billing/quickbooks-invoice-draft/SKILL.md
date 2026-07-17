---
name: QuickBooks Invoice Draft
description: When time-and-materials work on a ticket is done and you want a QuickBooks invoice drafted from its time entries — created as a draft, never sent.
category: Finance & Billing
tools: [search_tickets, search_clients, add_ticket_note]
connectors: [Zapier: QuickBooks]
---

# QuickBooks Invoice Draft

**When to use:** "Invoice the T&M work on this ticket in QuickBooks" / "draft an invoice from the time entries on <ticket>" / "bill out last week's project tickets for <client> — drafts only."

## Prompt

```
Build a QuickBooks invoice from a ticket's logged time entries for T&M work — line items from
real entries, matched to the right QuickBooks customer — created as a DRAFT for a human to
review and send. Zapier actions used: `Zapier: QuickBooks "Find Customer"`, `Zapier:
QuickBooks "Create Invoice"`.

1. Pull the ticket(s) with full time entries via search_tickets. Confirm with me that this
   work is T&M/billable outside a fixed-fee agreement — if coverage is unclear, stop and route
   to the scope question first (Out-of-Scope Billing Flag); never invoice work that might be
   agreement-covered.

2. Build the line items strictly from time entries: date, tech, duration, entry description,
   billable flag. Include only entries marked billable. Rate: use the rate I specify (or the
   standard T&M rate) — never guess a rate.

3. Show the draft to me in chat first: customer, line items, quantities, rate, subtotal, and
   which entries were excluded (non-billable or zero-duration) and why. Get an explicit "yes,
   create it" before touching QuickBooks.

4. Resolve the customer with `Zapier: QuickBooks "Find Customer"` using the client's billing
   name from search_clients. Exact/unambiguous match required — if multiple or zero
   candidates, list them and ask; never attach an invoice to a guessed customer.

5. Create the invoice with `Zapier: QuickBooks "Create Invoice"` as a DRAFT. Do NOT use any
   send action — no "Send Invoice", no email delivery, ever, regardless of how the request is
   phrased.

6. Post a plain-text note via add_ticket_note: invoice drafted in QuickBooks, date range and
   hours covered, awaiting review by me. Output the recap with the invoice reference.

Guardrails: money-moving actions are draft + approval-gated — this skill creates draft invoices
only and NEVER sends one; if asked to send, decline and hand off (sending is a deliberate human
act in QuickBooks). Time-entry evidence is the source of truth: every line item traces to a
real entry — no estimated, rounded-up, or "remembered" hours; if entries are missing, fix time
capture first (Time Entry Revenue Audit). Never state the work is billable per the agreement
without agreement evidence — my confirmation is recorded in the note as the authorization.
Confidence gate on customer matching: exact match or ask. Requires the member's Zapier
connector with QuickBooks connected; if unavailable, degrade to producing the invoice as a
structured text block for manual entry. One ticket's work per invoice unless I explicitly ask
to combine; never merge multiple clients into one invoice under any circumstances.
```
