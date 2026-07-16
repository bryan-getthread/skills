---
name: Zapier Shopify Clients
description: For retail clients running on Shopify — pull order/store status context (read-only) onto "the store is down" and "orders aren't coming through" tickets, so the desk diagnoses with evidence instead of the client's panic. Use for "check their Shopify — is it actually broken", "when did orders stop", or verifying order flow after a fix.
category: Connectors
tools: [search_tickets, search_clients, add_ticket_note, web_search, "zapier: Shopify"]
---

# Zapier Shopify Clients

"The store is down" arrives with zero diagnostics and maximum urgency. For clients on Shopify, the first evidence is in the store itself: are orders flowing, when did the last one land, is checkout the problem or is it DNS/theme/app-stack? This skill pulls read-only order and store context onto the ticket to separate "Shopify is fine, something in front of it is broken" from "order flow genuinely stopped at <time>" — and it never touches the store's commerce data.

## When to use

- "<client> says their store is down / customers can't order — check what Shopify shows."
- "When did their last order come through?" — establishing the failure window.
- A revenue-impacting ticket needs order-flow evidence before or after a fix.
- Post-fix verification: "confirm orders are flowing again."

## Steps

1. **Verify before promising (Shopify's Zapier inventory is not pre-verified in this library):** run the Zapier Action Discovery pattern first — confirm read/find actions for orders (and ideally products/store info) exist and are enabled. Shopify's Zapier surface includes writes (create orders/products) this skill must not use; note for the member that only the find/read actions are in scope. No order-lookup action → the evidence must come from the client's admin; say so and degrade.
2. Anchor from the ticket: the client's store, the reported symptom, and when it reportedly started. Confirm the right store where the client runs several.
3. **Establish the order-flow timeline:** find recent orders sorted by recency — last order timestamp, rough cadence before the reported window vs since. Orders flowing normally through "down" → the storefront path (DNS, theme, an app, payments) is the suspect, not order processing; recent gap versus their normal cadence → note the gap start as the failure window. Be cadence-honest: a store that sells nine orders a day proves nothing by one quiet hour.
4. Pull the specific evidence the symptom calls for, read-only: a named customer-order dispute → find that order's status/fulfillment state; "checkout errors" → whether any orders completed since the reports began; "site down" → whether *anything* has reached Shopify at all in the window.
5. `add_ticket_note` (plain text): findings with timestamps and "per the store's Shopify data, as of <date>" provenance — last order time, cadence comparison, and the diagnostic conclusion it supports ("order flow stopped ~14:20" or "orders flowing; storefront path suspected"). Facts with provenance, not certainty the data doesn't carry.
6. Route the diagnosis: Shopify-platform suspicion → check Shopify's public status page (`web_search`) and cite it; something-in-front suspicion → hand the ticket the narrowed scope (DNS/theme/app/payment) for normal troubleshooting.
7. Post-fix verification on request: re-check for new orders since the fix time and note the observation — "orders resumed at <time>" is the closure evidence a revenue ticket deserves.

## Guardrails

- **Member-connected Zapier required** (the client's Shopify store, standing client-granted access). Degrade: tell the member exactly what to ask the client to check in Shopify admin (last order time, recent order count, checkout test), and note on the ticket that store-side evidence is pending.
- **Read-only stance, absolute:** this skill never creates, edits, cancels, fulfills, or refunds anything — no orders, no products, no discounts, no customer records. The store is the client's revenue; the desk looks, the desk does not touch. If a fix requires a store-side change, that is the client's (or their developer's) action to take, documented on the ticket.
- Their customers' data is third-party data: extract order timestamps, statuses, and counts — not customer names, addresses, or payment details, except the minimum for a named-order dispute, and never onto broadly-visible notes.
- Cadence honesty: always compare against the store's own normal rhythm before declaring flow stopped; low-volume stores make short windows meaningless, and the note must say so.
- No revenue claims: report order counts and gaps, never extrapolated "lost revenue" figures — that math belongs to the client.
- Task cost: each Zapier call = 2 Zapier tasks — a diagnosis pass is typically two or three finds; verification is one more. No polling the store for the next order.
