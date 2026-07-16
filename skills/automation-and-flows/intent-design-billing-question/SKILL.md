---
name: Billing Question Intent Design
description: Build the billing-question intent — route invoice and payment questions to the right human with account context attached, while the intent itself never discusses amounts, balances, or payment status. Use when asked to "build a billing intent", "route billing questions", or when billing asks show up in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets]
---

# Billing Question Intent Design

Guide the admin through building a billing intent that is deliberately a router, not an answerer: it recognizes invoice/payment/contract questions, captures just enough context, and lands them with the person who owns billing — because a support agent quoting a wrong amount is worse than no automation at all.

## When to use

- "Build an intent for billing questions."
- "Invoice questions hit the tech queue and bounce around for days."
- Intent Mining shows billing/invoice chatter mixed into support volume.

## Design

**Trigger phrases (adapt to real ticket language):** "question about our invoice", "billing question", "why were we charged", "invoice seems wrong", "need a copy of the invoice", "update our payment method", "who do I talk to about billing", "charge on our statement", "contract renewal question", "pricing question", "cancel a service" (billing-adjacent — routes here with a retention flag).
Near-miss watch: "license cost for new software" belongs to the software-install intent's approval path; "quote for a laptop" is the hardware-request intent.

**Arguments to collect (context, not financials):**
- Requester name and role (billing conversations should be with authorized contacts; capture, don't police — the human verifies).
- Topic class: invoice question / payment method / copy of invoice / contract-renewal / possible-cancellation / other.
- Invoice number or month in question, if they have it (reference only — the intent never looks amounts up).
- Preferred callback method.
- Urgency (payment blocked vs. general question).

**Reply flow:**
1. Recognize and classify; collect the arguments.
2. Reply that the billing team/owner will follow up, with the client's stated response window ONLY if the admin provided one — never improvise a timeframe.
3. Create a ticket routed to the billing board/owner (not the tech queue) with the context block; possible-cancellation topics get flagged for the account owner as retention-sensitive and are handled with extra care in tone — thank them, no pushback, no terms discussion.
4. **The intent never states, confirms, or estimates any amount, balance, discount, or payment status, and never processes payment details in chat.** If the user posts card numbers or bank details, the reply asks them not to share payment details in this channel and confirms the billing contact will collect them securely.

**Handoff rule:** every substantive answer is human. The intent's entire job is correct routing plus complete context.

**Variation hooks (per client — here, often per-MSP rather than per-end-client):** who owns billing, the billing board/queue, response-window wording, whether end-client users may ask billing questions at all or must go through their internal approver.

**Success metric:** routing accuracy — percentage of billing tickets landing on the billing owner without a tech touch — and time-to-first-human-response on billing topics. Deflection target is deliberately zero.

## Steps

1. `list_intents` — check for an existing billing intent; prefer updating.
2. `search_tickets` for billing-ish tickets in the support queue; mine phrasing and measure the current misroute pain (baseline for the metric).
3. Confirm with the admin who the billing owner/route is — this is the one argument the skill cannot infer.
4. Draft the full spec: triggers, context arguments, router flow, the no-amounts rule verbatim in the reply guidance, variations. Test plan: 5 should-match, 3–5 should-not (software-license and hardware-quote near-misses).
5. Show the draft; on explicit confirmation call `create_intent` then the variation tools.
6. Report what was created, restate the test plan, recommend activation after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, output the complete written spec for an admin to apply.
- Absolute rule: no amounts, balances, charges, discounts, payment statuses, or contract terms in any reply — not even to confirm what the user said. Route, don't discuss.
- No payment-detail collection in chat; redirect to the billing contact's secure process.
- Cancellation-flavored messages get flagged retention-sensitive and routed to the account owner; the reply stays warm and commitment-free.
- Never improvise response windows or billing policy; placeholder until the admin supplies wording.
- Confirm before any write; never activate on the member's behalf. Context block in plain text.
