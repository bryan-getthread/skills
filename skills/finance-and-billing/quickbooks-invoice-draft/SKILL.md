---
name: QuickBooks Invoice Draft
description: When time-and-materials work on a ticket is done and you want a QuickBooks invoice drafted from its time entries — created as a draft, never sent.
category: Finance & Billing
tools: [search_tickets, search_clients, add_ticket_note, zapier]
---

# QuickBooks Invoice Draft

Build a QuickBooks invoice from a ticket's logged time entries for T&M work — line items from real entries, matched to the right QuickBooks customer — created as a draft for a human to review and send.

Zapier actions used: `zapier: QuickBooks "Find Customer"`, `zapier: QuickBooks "Create Invoice"`.

## When to use

- "Invoice the T&M work on this ticket in QuickBooks."
- "Draft an invoice from the time entries on <ticket>."
- "Bill out last week's project tickets for <client> — drafts only."

## Steps

1. Pull the ticket(s) with full time entries via `search_tickets`. Confirm with the requester that this work is T&M/billable outside a fixed-fee agreement — if coverage is unclear, stop and route to the scope question first (Out-of-Scope Billing Flag); never invoice work that might be agreement-covered.
2. Build the line items strictly from time entries: date, tech, duration, entry description, billable flag. Include only entries marked billable. Rate: use the rate the requester specifies (or their standard T&M rate) — never guess a rate.
3. Show the draft to the requester in chat first: customer, line items, quantities, rate, subtotal, and which entries were excluded (non-billable or zero-duration) and why. Get an explicit "yes, create it" before touching QuickBooks.
4. Resolve the customer with `zapier: QuickBooks "Find Customer"` using the client's billing name from `search_clients`. Exact/unambiguous match required — if multiple or zero candidates, list them and ask; never attach an invoice to a guessed customer.
5. Create the invoice with `zapier: QuickBooks "Create Invoice"` as a draft. Do NOT use any send action — no "Send Invoice", no email delivery, ever, regardless of how the request is phrased.
6. Post a plain-text note via `add_ticket_note`: invoice drafted in QuickBooks, date range and hours covered, awaiting review by <requester>. Output the recap with the invoice reference.

## Guardrails

- Money-moving actions are draft + approval-gated: this skill creates draft invoices only and NEVER sends one. If asked to send, decline and hand off to the human — sending is a deliberate human act in QuickBooks.
- Time-entry evidence is the source of truth: every line item traces to a real entry. No estimated, rounded-up, or "remembered" hours; if entries are missing, fix time capture first (Time Entry Revenue Audit).
- Never state the work is billable per the agreement without agreement evidence — the requester's confirmation is recorded in the note as the authorization.
- Confidence gate on customer matching: exact match or ask. A misdirected invoice is worse than a delayed one.
- Requires the member's Zapier connector with QuickBooks connected; if unavailable, degrade to producing the invoice as a structured text block the requester can enter manually.
- One ticket's work per invoice unless the requester explicitly asks to combine; never merge multiple clients into one invoice under any circumstances.
