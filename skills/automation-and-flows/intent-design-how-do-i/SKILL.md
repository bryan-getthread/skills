---
name: How-Do-I Self-Help Router Intent Design
description: Build the catch-all "how do I…" self-help router — classify the how-to, serve the matching end-user guide or KB article, and escalate only when no guidance exists. Use when asked to "build a how-to intent", "deflect how-do-I questions", or when general how-to queries dominate Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets, search_knowledge_base]
---

# How-Do-I Self-Help Router Intent Design

Guide the admin through building the catch-all self-help router: it recognizes generic "how do I…" questions, classifies the topic, serves the matching end-user guide or KB article to deflect, and escalates to a human only when there is genuinely no guidance. It is the safety net beneath the specific intents — deliberately broad, but it must yield to more specific intents rather than swallow them.

## When to use

- "Build a general how-to / self-help intent."
- "We get a long tail of one-off how-do-I questions with no dedicated intent each."
- Intent Mining shows a large low-frequency how-to tail that no single intent covers.

## Design

**Trigger phrases (adapt to real ticket language):** "how do I…", "how to…", "where do I find…", "can you show me how to…", "what's the steps to…", "I don't know how to…", "is there a guide for…", "how do I set up…".
Collision rule (critical): this intent must sit BELOW the specific intents in priority. "How do I reset my password" belongs to password-reset; "how do I book a room" belongs to room-booking. Keep this router's job to the topics NOT covered by a dedicated intent — test that specific-intent utterances do NOT get captured here.

**Arguments to collect (minimal — just enough to classify and search):**
- The topic / what they're trying to do, in their words.
- Which app or system it concerns, if stated.
- Whether they've already tried a guide that didn't help (raises escalation priority).

**Reply flow (deflect-first via KB):**
1. Classify the how-to and `search_knowledge_base` for a matching end-user guide.
2. If a good match is found → reply with the article (link + a one-line summary of the relevant steps) and ask "did that answer it?". A confirmed yes is a deflection.
3. If no match, or the user says the guide didn't help → create a ticket capturing the topic, the app, and what they've tried, and route to the general helpdesk queue. Flag topics that recur with no article as KB-gap candidates.
4. Never fabricate steps or invent an article link — if the KB has nothing, escalate rather than inventing guidance.

**Handoff rule:** the router only serves existing guidance and captures intake — it does not perform the task or improvise procedures. Anything requiring an action on the user's account or device is escalated.

**Variation hooks (per client):** which KB/doc source to search, the client's escalation queue, topics the client wants always-escalated (never self-served), branding/tone of the reply.

**Success metric:** deflection rate — how-to conversations resolved by an article without a ticket. Watch two guardrail metrics alongside it: false-capture rate (specific-intent questions wrongly landing here) and the KB-gap list (repeated no-match topics that should become articles or dedicated intents).

## Steps

1. `list_intents` — review ALL existing intents first; this router must be scoped to avoid overlapping their triggers, and set to lower priority so specific intents win.
2. `search_tickets` for the how-to tail; cluster the topics to see what recurs and whether any cluster deserves its own intent instead of living in the catch-all.
3. `search_knowledge_base` to confirm which clustered topics actually have end-user guides — that's the deflectable set; the rest defines the escalation and KB-gap paths.
4. Draft the full spec: broad-but-scoped triggers, minimal arguments, the KB-match→deflect / no-match→escalate flow, routing target, variations, and a test plan (5 generic how-tos that should match, 3–5 specific-intent utterances that should NOT). Show before any write.
5. On explicit confirmation: `create_intent`, then `set_variation_arguments` / `set_variation_replies` / `update_variation`; confirm the priority ordering keeps specific intents ahead of it.
6. Report what was created, restate the test plan and the priority note, recommend activation only after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, degrade to a complete written spec for an admin to apply.
- Priority discipline: this catch-all must never outrank a specific intent. The dominant failure mode is swallowing questions that belong elsewhere — test specific-intent near-misses hard.
- Never fabricate steps or invent a KB link; if there's no article, escalate. No-match topics feed the KB-gap list, not an improvised answer.
- The router serves guidance and captures intake only — it never performs the task or acts on an account/device.
- Do not invent the client's KB source or escalation queue — placeholder anything unknown and flag it before activation.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text (PSA-safe, no markdown).
