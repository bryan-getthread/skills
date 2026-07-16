---
name: New Hire Intent Design
description: Build the new-hire onboarding intake intent — collect the full checklist up front so the ticket arrives complete and routes straight into the onboarding workflow. Use when asked to "build a new hire intent", "automate onboarding requests", or when new-user setup ranks high in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets]
---

# New Hire Intent Design

Guide the admin through building a new-hire intent whose whole job is complete intake: gather every checklist field in one conversation so the onboarding ticket never bounces back with "what's their start date?". This intent does not deflect — it makes the ticket dispatch-ready on arrival.

## When to use

- "Build an intent for new employee onboarding requests."
- "New-user tickets always come in half-filled — fix the intake."
- Intent Mining flagged new user / new hire as a top-volume request.

## Design

**Trigger phrases (adapt to real ticket language):** "new hire starting", "new employee", "onboard a new user", "set up a new user", "new starter", "we hired someone", "need accounts for a new person", "new team member starting Monday", "add a user", "employee starting next week".
Near-miss watch: "add a user to <distribution list>" belongs to the access-request intent, not this one.

**Arguments to collect (the checklist):**
- Full name and personal or manager contact for credential delivery.
- Start date (drives urgency and licensing timing).
- Job title / department / manager (drives group memberships and mirroring).
- "Model after" existing user, if the client works that way (placeholder: <user>).
- Equipment needed (laptop/desktop/phone) and work location (office/remote).
- Applications and licenses beyond the standard stack.
- Who approved the hire (authorization anchor for the request).

**Reply flow:**
1. Collect the arguments; confirm the full summary back to the requester.
2. Create the ticket with every field in a structured plain-text block and route to the onboarding board/workflow the client uses.
3. Reply with what happens next and that the assigned technician will confirm timelines — do not promise a completion date.

**Handoff rule:** account creation, licensing, and hardware are always human/workflow actions — the intent only performs intake. If the requester asks for credentials in-chat, state they will be delivered through the client's secure method.

**Variation hooks (per client):** checklist fields that differ (some clients need badge/door access, some need LOB app seats), standard hardware bundles, whether "model after user" is allowed, which board or flow receives the ticket.

**Success metric:** first-touch completeness — percentage of new-hire tickets that reach a technician with zero follow-up questions needed. Secondary: time from request to ticket creation. Deflection is not the goal here.

## Steps

1. `list_intents` — check for an existing new-user/onboarding intent; prefer `update_intent` over a duplicate.
2. `search_tickets` for recent new-hire tickets; mine both the users' phrasing (triggers) and the follow-up questions techs had to ask (those become arguments).
3. Draft the full spec: triggers, the complete argument checklist, confirmation reply, routing target, variations. Include a test plan (5 should-match, 3–5 should-not, including an access-request near-miss).
4. Show the draft; on explicit confirmation call `create_intent` then `set_variation_arguments` / `set_variation_replies` / `update_variation`.
5. Report what was created, restate the test plan, recommend activation only after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, degrade to a complete written spec for an admin to apply.
- Never design replies that deliver credentials in the conversation; credential delivery follows the client's secure process.
- Do not invent the client's standard hardware, license stack, or onboarding SLA — placeholder anything unknown and flag it before activation.
- Every argument adds friction: keep the checklist to what the onboarding workflow genuinely consumes; push rarely-used fields into a variation.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text (PSA-safe, no markdown).
