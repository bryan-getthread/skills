---
name: Intent Builder
description: Build or update a customer-facing intent — trigger phrases, arguments, replies, and variations — with a test plan before anything goes live. Use when asked to "create an intent", "add trigger phrases", "update the replies for an intent", or after Intent Mining picks a candidate.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets]
---

# Intent Builder

Take an intent idea from concept to a fully specified, tested draft: trigger phrases grounded in real ticket language, the arguments to collect, the replies per variation, and a test plan — activated only after the admin explicitly says go.

## When to use

- "Build an intent for password reset requests."
- "Add more trigger phrases to the new-user intent — it keeps missing."
- "Change what the office-move intent asks for."

## Steps

1. Call `list_intents` and check for overlap. If a similar intent exists, propose updating it (`get_intent` → `update_intent`) instead of creating a near-duplicate.
2. Ground the language: `search_tickets` for 5–10 real examples of the request and mirror the end users' actual phrasing in the trigger set.
3. Draft the full spec and show it before touching any write tool:
   - **Trigger phrases** — 8–15, varied in vocabulary and length; include misspellings and short forms users actually type.
   - **Arguments** — only what the workflow genuinely needs (who, what system, when). Every argument adds friction.
   - **Replies per variation** — plain, localizable language; no idioms, no fabricated policy answers. If a reply states a policy (turnaround time, approval requirement), it must come from the member, not be invented.
4. Include a **test plan** in the draft: 5 utterances that should match, and 3–5 near-miss utterances that should NOT match (to catch collisions with other intents).
5. On confirmation only: `create_intent` (or `update_intent`), then `set_variation_arguments`, `set_variation_replies`, and `update_variation` as needed.
6. Report back exactly what was created/changed and restate the test plan for the member to verify in a test conversation. Do not activate the intent — recommend the member activate it after the test utterances pass.

## Guardrails

- Intent tools are admin-only; if they are absent from the tool list, the member lacks admin rights — output the full spec for an admin to apply instead.
- Never create or update without showing the complete draft and getting explicit confirmation. Never activate on the member's behalf.
- Check the "should NOT match" list against existing intents' trigger phrases; flag any collision before creating.
- Replies are customer-facing: no internal jargon, no client or staff names, no promises the member did not approve.
