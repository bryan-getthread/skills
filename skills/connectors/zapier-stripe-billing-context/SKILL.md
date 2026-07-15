---
name: Zapier Stripe Billing Context
description: Read-only Stripe check on a client's payment health — failed charges, unpaid invoices, subscription state — before renewing services, dispatching billable work, or answering "are they current?". Use for "check <client>'s Stripe standing", "did their last payment go through", or pre-dispatch billing awareness when the MSP bills via Stripe.
category: Connectors
tools: [search_tickets, search_clients, add_ticket_note, "zapier: Stripe"]
---

# Zapier Stripe Billing Context

Before the desk renews a service, ships hardware, or dispatches billable hours, it's worth thirty seconds to know whether the client's payments are actually landing. This skill reads Stripe — find the customer, their subscription state, recent failed charges and unpaid invoices — and reports standing as decision input. Read-only stance: it looks at money; it never moves, requests, or configures it.

## When to use

- "Check <client>'s Stripe standing before we dispatch the project work."
- "Their service auto-renews next week — did the last payment succeed?"
- A renewal or expansion conversation where payment health changes the play.

## Steps

1. Resolve the Stripe customer: find-customer matched from the client record (`search_clients`) — email/domain over name. Zero or multiple matches → stop and report; standing read off the wrong customer is misinformation with money attached.
2. Pull the health picture, scoped and minimal:
   - Subscription state (active / past_due / canceled) for the services in question.
   - Recent failed charges and open/unpaid invoices — count, total, oldest age.
   - Nothing else: no card details beyond what the tool surfaces as status, no unrelated payment history.
3. Report with provenance and neutrality: "per Stripe as of now — subscription past_due, 2 failed charges in 30 days totaling <amount>, oldest unpaid invoice <n> days." Facts, not character judgments about the client.
4. Route the finding to the decision-maker: `add_ticket_note` (plain text) when a ticket drove the check, so whoever approves the dispatch/renewal has it. The skill informs the decision; it never blocks work itself, and it **never raises payment status with the client** — arrears conversations belong to the business owner.
5. If the member then wants action (retry a charge, send a payment link, change a subscription), that is outside this skill's stance — hand it to the human in the Stripe dashboard, or to an explicitly approved separate workflow. Name the boundary rather than quietly crossing it.

## Guardrails

- **Member-connected Zapier required** (the MSP's Stripe account). Degrade: state that Stripe wasn't checked and let the decision proceed on other information — never guess payment standing.
- Read-only is the stance, not a suggestion: no charges, refunds, payment links, checkout sessions, or subscription changes from this skill, even though the connector offers them. Money moves by human hands.
- No confident customer match → no report; say so.
- Payment data is among the most sensitive context the desk touches: minimum scope, ticket notes carry standing summaries only (state, counts, ages — amounts when the decision needs them), and nothing client-facing ever includes it.
- Figures come only from tool results, stamped "as of" — a standing check is a snapshot, not a promise about tomorrow's retry.
- Task cost: each Zapier call = 2 tasks — one find-customer + one or two targeted reads is the expected shape.
