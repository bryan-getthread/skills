---
name: Hardware Request Intent Design
description: Build the hardware-procurement intake intent — specs, budget approver, and needed-by date captured up front so quotes go out once, not after three follow-ups. Use when asked to "build a hardware request intent", "automate equipment requests", or when procurement asks rank high in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets]
---

# Hardware Request Intent Design

Guide the admin through building a hardware-request intent that captures what is needed, who pays, and by when — so the procurement ticket arrives quotable and the desk never becomes the bottleneck between a request and a purchase order.

## When to use

- "Build an intent for new equipment requests."
- "Hardware asks come in as 'need a laptop' with nothing else."
- Intent Mining flagged equipment/procurement requests as recurring volume.

## Design

**Trigger phrases (adapt to real ticket language):** "need a new laptop", "order a monitor", "request new equipment", "new computer for", "need a docking station", "replace my laptop", "buy a keyboard", "hardware request", "need a second screen", "get a phone for".
Near-miss watch: "my laptop is broken" is a support/repair ticket first (repair-vs-replace is a tech's call); "new hire needs a laptop" belongs in the new-hire intent's checklist.

**Arguments to collect:**
- What device/peripheral, for whom (<user>), and how many.
- Use case or specs: standard build vs. special needs (CAD, dual 4K, travel weight). "Standard for their role" is a valid answer.
- **Budget approver** — the named person who signs off spend for this client (drives the approval step).
- **Needed-by date** — drives shipping choices and flags unrealistic expectations early.
- Replacement or addition; if replacement, what happens to the old unit.
- Delivery location.

**Reply flow:**
1. Collect arguments; confirm the summary.
2. If the request matches the client's pre-approved standard catalog → note that in the ticket (may skip quoting).
3. Create the ticket routed to the procurement board/workflow with a plain-text field block; if the client requires approval before quoting, the ticket carries the approver for the desk's approval step (pairs with send_approval on the desk side).
4. Reply with the expected next step (quote/approval) — never quote prices, never promise availability or delivery dates.

**Handoff rule:** all purchasing, quoting, and pricing is human. The intent must never state prices, stock, or delivery estimates — supply chains change and the intent's word becomes a commitment.

**Variation hooks (per client):** standard hardware catalog, who the budget approver is, whether pre-approved items skip approval, procurement lead-time wording, old-device return process.

**Success metric:** first-touch quotability — percentage of hardware tickets a procurement person can quote without asking anything. Secondary: request-to-quote cycle time.

## Steps

1. `list_intents` — check for an existing hardware/procurement intent; prefer updating.
2. `search_tickets` for recent hardware requests; mine phrasing and the questions the procurement team had to ask (argument candidates); note recurring standard items (catalog/variation seed).
3. Draft the full spec: triggers, arguments, approval-aware routing, variations. Test plan: 5 should-match, 3–5 should-not (broken-device repair and new-hire near-misses).
4. Show the draft; on explicit confirmation call `create_intent` then the variation tools.
5. Report what was created, restate the test plan, recommend activation after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, output the complete written spec for an admin to apply.
- Never state or estimate prices, stock, lead times, or delivery dates in replies; procurement humans own commitments.
- Budget-approver capture is mandatory in the design — a hardware ticket without an approver is the follow-up loop this intent exists to kill.
- Do not invent the client's standard catalog or approval policy; placeholder and flag before activation.
- Confirm before any write; never activate on the member's behalf. Field block in plain text.
