---
name: Distribution List Change Intent Design
description: Build the distribution-list add/remove intent — capture the list, the member, the direction, and the list-owner's approval so the change ticket arrives ready to action. Use when asked to "build a distribution list intent", "automate DL requests", or when DL changes rank high in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets]
---

# Distribution List Change Intent Design

Guide the admin through building an intent that captures a clean distribution-list (DL) change — who goes on or off which list, and who approved it — so the ticket never bounces for missing detail. The intent NEVER edits list membership itself; it captures and routes with the owner's approval attached.

## When to use

- "Build an intent for distribution list changes."
- "DL requests come in without saying who approved them — fix the intake."
- Intent Mining flagged add/remove-from-group as a recurring request.

## Design

**Trigger phrases (adapt to real ticket language):** "add me to the distribution list", "remove me from the all-staff list", "add someone to a group email", "take me off the sales DL", "update the distribution group", "who's on the marketing list", "add to email group", "unsubscribe me from the internal list".
Near-miss watch: "shared mailbox access" belongs to the shared-mailbox intent; "security group / app access" belongs to the access-request intent — keep triggers scoped to email distribution lists.

**Arguments to collect:**
- Which distribution list (address or exact name).
- Direction: add or remove.
- Which member(s) — self or named others.
- Who owns or approves that list (authorization anchor — required for add, and for removing anyone other than self).

**Reply flow:**
1. Collect the arguments; confirm list, direction, and member(s) back to the requester.
2. If the change is approved (list owner named, or self-removal where the client permits it) → create the ticket with a structured plain-text block and route to the access/M365 workflow.
3. If approval is missing → ask for the list owner's sign-off before creating the ticket.

**Handoff rule (non-negotiable):** the intent never adds or removes members — DL membership is always an approved human/workflow action. State that the change follows owner approval.

**Variation hooks (per client):** which lists are self-service vs. owner-approved, whether self-removal is always allowed, the owner map per list, which board or flow receives the ticket.

**Success metric:** first-touch completeness — percentage of DL-change tickets arriving with list, direction, member, and approval captured (zero follow-up questions).

## Steps

1. `list_intents` — check for an existing access/group/DL intent; prefer `update_intent` over a duplicate.
2. `search_tickets` for recent DL-change tickets; mine phrasing (triggers) and the follow-ups techs asked (usually approver and exact list name → arguments).
3. Draft the full spec: triggers, arguments, confirmation reply, routing target, variations, and a test plan (5 should-match, 3–5 should-not including a shared-mailbox near-miss). Show before any write.
4. On explicit confirmation: `create_intent`, then `set_variation_arguments` / `set_variation_replies` / `update_variation`.
5. Report what was created, restate the test plan, recommend activation only after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, degrade to a complete written spec for an admin to apply.
- The intent captures and routes — it NEVER changes list membership. Changes follow list-owner approval through the client's process.
- Require owner approval for adds and for removing anyone other than the requester; self-removal is allowed only where the client's policy permits.
- Do not invent list owners or the client's DL policy — placeholder anything unknown and flag it before activation.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text (PSA-safe, no markdown).
