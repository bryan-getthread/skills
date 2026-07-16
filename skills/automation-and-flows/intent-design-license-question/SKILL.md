---
name: License Question Intent Design
description: Build the "do I have a license for X" intent — a router that captures the app and account context and routes to licensing, never quoting a price. Use when asked to "build a license question intent", "handle 'do I have a license' requests", or when licensing questions show up in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets, search_knowledge_base]
---

# License Question Intent Design

Guide the admin through building a router intent for "do I have a license for X?" questions. The intent captures which product and enough account context to answer, then routes to the licensing/procurement path. It NEVER quotes a price and never commits the client to a purchase — pricing and buying decisions belong to a human.

## When to use

- "Build an intent for license questions."
- "Users keep asking whether they have a Visio/Project/Adobe license."
- Intent Mining flagged 'do I have a license' / 'can I get access to <app>' questions.

## Design

**Trigger phrases (adapt to real ticket language):** "do I have a license for X", "can I get Visio", "am I licensed for Project", "need Adobe Acrobat", "is <app> included in my plan", "request a license", "which Office plan do I have", "do we pay for <tool>".
Near-miss watch: "install <app> I'm already licensed for" belongs to the software-install intent; "billing/invoice question" belongs to the billing intent — keep triggers scoped to *whether a license exists / can be obtained*.

**Arguments to collect (enough to route and answer):**
- Which product/edition (e.g. "Visio Plan 2", "Adobe Acrobat Pro").
- Who it's for (self or named user) and their department/role (some licenses are role-gated).
- Whether they already have any edition of it (upgrade vs. net-new).
- Business justification, if the client requires one for paid seats.

**Reply flow (route-first, price-silent):**
1. If the answer is knowable from the client's standard entitlement (e.g. the app is in everyone's plan) → reply that it's included and route to the software-install path or KB. This can be a deflection.
2. If it needs a paid seat or a check against the tenant's license pool → create a ticket with the product, the user, and the justification, and route to the licensing/procurement workflow. Tell the user the team will confirm availability and any approval needed.
3. Never state or estimate a cost, per-seat price, or renewal figure — that comes from procurement.

**Handoff rule (non-negotiable):** the intent never quotes price, never assigns a paid license, and never authorizes a purchase. It answers "is this included?" where certain, otherwise it routes with full context. Assigning seats and approving spend are human/workflow actions.

**Variation hooks (per client):** the standard entitlement/plan (what's included for everyone), which apps are role-gated, whether paid seats need manager/finance approval, the licensing/procurement routing target.

**Success metric:** routing accuracy — percentage of license questions sent to the right path (included→install vs. paid→procurement) with the product and user captured. Secondary: deflection rate on the "it's already included" branch.

## Steps

1. `list_intents` — check for an existing license/software intent; prefer `update_intent` and avoid overlap with the software-install and billing intents.
2. `search_tickets` for past license questions; note which apps are commonly asked about and which are actually included vs. paid — that shapes the branches.
3. `search_knowledge_base` for the client's standard license/entitlement documentation; reference it in the "it's included" reply.
4. Draft the full spec: triggers, arguments, branch replies, routing target, variations, and a test plan (5 should-match, 3–5 should-not including a software-install and a billing near-miss). Show before any write.
5. On explicit confirmation: `create_intent`, then `set_variation_arguments` / `set_variation_replies` / `update_variation`.
6. Report what was created, restate the test plan, recommend activation only after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, degrade to a complete written spec for an admin to apply.
- NEVER quote or estimate a price, per-seat cost, or renewal figure — pricing and purchase authorization are human/procurement decisions.
- The intent answers "is it included?" only where the entitlement is certain; when unsure it routes rather than guessing.
- Do not invent the client's plan/entitlement or approval policy — placeholder anything unknown and flag it before activation.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text (PSA-safe, no markdown).
