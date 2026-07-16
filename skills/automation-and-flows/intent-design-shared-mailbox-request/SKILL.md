---
name: Shared Mailbox Request Intent Design
description: Build the shared-mailbox-access intent — capture which mailbox, the access level, and who approves, so the ticket arrives grant-ready. Use when asked to "build a shared mailbox intent", "automate mailbox access requests", or when mailbox-access ranks high in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets]
---

# Shared Mailbox Request Intent Design

Guide the admin through building an intent that turns "I need access to the sales inbox" into a complete, approver-attached ticket. The intent NEVER grants access — it captures the resource, the access level, and the authorization so a technician (or workflow) can act without a round-trip.

## When to use

- "Build an intent for shared mailbox access requests."
- "People keep asking for mailbox access with no approver — fix the intake."
- Intent Mining flagged shared/team mailbox access as a recurring request.

## Design

**Trigger phrases (adapt to real ticket language):** "access to a shared mailbox", "add me to the shared inbox", "need to see the sales mailbox", "give me access to info@", "shared mailbox permission", "add me to the team inbox", "can't see the shared mailbox", "send-as for the support mailbox".
Near-miss watch: "set up my own mailbox" is a new-user/mailbox-provisioning request; "distribution list" belongs to the distribution-list intent — keep triggers scoped to *shared mailbox* access.

**Arguments to collect:**
- Which shared mailbox (the address or exact display name).
- Access level requested: read/full access, send-as, and/or send-on-behalf.
- Who owns or approves that mailbox (authorization anchor — required).
- Duration if temporary (some access is for coverage only) — variation hook.

**Reply flow:**
1. Collect the arguments; confirm the mailbox and access level back to the requester.
2. If the approver is named → create the ticket with a structured plain-text block and route to the access/M365 workflow the client uses; tell the requester approval will be confirmed before access is granted.
3. If no approver is known → ask who owns the mailbox before creating the ticket; do not proceed on an unauthorized request.

**Handoff rule (non-negotiable):** the intent never grants, modifies, or confirms permissions — mailbox access is always an approved human/workflow action. State that access follows owner approval; never imply it is already done.

**Variation hooks (per client):** which approver/owner map applies to which mailbox, whether the client allows temporary/coverage access, which board or flow receives the ticket, whether send-as requires extra sign-off.

**Success metric:** first-touch completeness — percentage of mailbox-access tickets that reach the technician with mailbox, access level, and named approver already captured (zero follow-up questions). Deflection is not the goal.

## Steps

1. `list_intents` — check for an existing access or mailbox intent; prefer `update_intent` over a duplicate.
2. `search_tickets` for recent mailbox-access tickets; mine the users' phrasing (triggers) and the follow-ups techs had to ask (those become arguments — usually the approver and the exact access level).
3. Draft the full spec: triggers, arguments, confirmation reply, routing target, variations, and a test plan (5 should-match, 3–5 should-not including a distribution-list near-miss). Show it before any write.
4. On explicit confirmation: `create_intent`, then `set_variation_arguments` / `set_variation_replies` / `update_variation` per client variation.
5. Report what was created, restate the test plan, recommend activation only after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent from the tool list, output the complete written spec for an admin to apply instead.
- The intent captures and routes — it NEVER grants access. Access always follows owner/approver authorization through the client's process.
- Never proceed without a named approver; an access request with no authorization is incomplete, not a ticket to fulfill.
- Do not invent the mailbox owner or the client's approval policy — placeholder anything unknown and flag it before activation.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text (PSA-safe, no markdown).
