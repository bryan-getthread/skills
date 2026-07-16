---
name: Travel Notice Intent Design
description: Build the travel-notice intent — capture who is travelling, where, and when so conditional-access and MFA behavior can be anticipated before the user hits a sign-in block abroad. Use when asked to "build a travel notice intent", "handle travel access heads-ups", or when travel-related lockouts show up in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets, search_knowledge_base]
---

# Travel Notice Intent Design

Guide the admin through building an intent that captures a heads-up when a user is about to travel — especially internationally — so conditional-access rules, MFA prompts, and roaming sign-ins can be anticipated instead of triaged as an emergency lockout mid-trip. The intent gathers travel context and routes it; it never changes security policy itself.

## When to use

- "Build an intent for travel notices."
- "Users keep getting locked out abroad because nobody told us they were travelling."
- Intent Mining surfaced travel-related access blocks or MFA-abroad tickets.

## Design

**Trigger phrases (adapt to real ticket language):** "I'm travelling next week", "going abroad for work", "heads up I'll be in <country>", "will I be able to log in overseas", "travelling internationally, any access issues", "working from another country", "on a business trip, need access", "MFA while abroad".
Near-miss watch: "I'm locked out right now" (already blocked) is a password/lockout or security issue, not a proactive notice — route those to the password-reset or security intent.

**Arguments to collect:**
- Who is travelling (self or named user).
- Destination(s) / region — drives whether conditional access geoblocks apply.
- Travel dates (start and return).
- Which devices and apps they'll need (corporate laptop, phone, VPN, specific SaaS).
- Whether they'll have their registered MFA method with them (phone/roaming) — the single biggest cause of abroad lockouts.

**Reply flow:**
1. Collect the arguments; confirm destination and dates back to the requester.
2. Share the client's proactive travel guidance if it exists (e.g. keep your MFA device, test VPN before you fly) from the knowledge base — do not invent steps.
3. Create the ticket with a structured plain-text block and route to the security/access team so they can review conditional-access impact ahead of the trip. Tell the user the team will confirm any action needed before departure.

**Handoff rule (non-negotiable):** the intent never modifies conditional-access rules, MFA settings, or geoblocking — those are security-team actions. It only captures the notice and routes it. Never tell a user their access is "cleared" for travel.

**Variation hooks (per client):** whether the client uses geo-based conditional access at all, VPN-before-travel requirements, the security-team routing target, region-specific policy notes.

**Success metric:** first-touch completeness — percentage of travel notices arriving with traveller, destination, dates, and MFA-device status captured, early enough for the team to act (zero follow-up questions). Secondary: reduction in abroad-lockout tickets.

## Steps

1. `list_intents` — check for an existing travel/access intent; prefer `update_intent` over a duplicate.
2. `search_tickets` for past abroad-lockout or travel tickets; mine phrasing (triggers) and the details that would have prevented the lockout (those become arguments).
3. `search_knowledge_base` for the client's travel/MFA-abroad guidance; reference it in the reply rather than restating steps that may drift.
4. Draft the full spec: triggers, arguments, reply, routing target, variations, and a test plan (5 should-match, 3–5 should-not including an active-lockout near-miss). Show before any write.
5. On explicit confirmation: `create_intent`, then `set_variation_arguments` / `set_variation_replies` / `update_variation`.
6. Report what was created, restate the test plan, recommend activation only after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, degrade to a complete written spec for an admin to apply.
- The intent never changes conditional access, MFA, or geoblocking — it captures and routes to the security team. Never confirm access is pre-cleared.
- Do not invent the client's conditional-access posture or travel policy — placeholder anything unknown and flag it before activation.
- Replies are customer-facing: plain, localizable language; no fabricated turnaround or "you're all set" promises.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text (PSA-safe, no markdown).
