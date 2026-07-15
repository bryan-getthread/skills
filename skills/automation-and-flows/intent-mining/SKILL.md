---
name: Intent Mining
description: Analyze recent tickets to find the top customer-facing intents worth building, ranked by volume and automatability, with draft trigger phrases — use when asked to "mine tickets for intents", "what intents should we build", or "analyze trends and create intents".
category: Automation & Flows
tools: [search_tickets, list_intents, list_boards, get_intent]
---

# Intent Mining

Mine a window of real tickets for repeated end-user requests that could become customer-facing intents, skip what already exists, and return a ranked build list with draft trigger phrases ready for Intent Builder.

## When to use

- "Analyze the last 30 days of tickets and tell me what intents to build."
- "What are our most common requests that Magic could deflect?"
- A partner wants to grow zero-touch resolution and needs a data-backed intent roadmap.

## Steps

1. Confirm the analysis window (default: last 30 days) and which boards to include (`list_boards`). Exclude alert/monitoring boards — machine-generated tickets are not intents.
2. Call `list_intents` first and note every existing intent and its trigger phrases. Anything already covered is excluded from recommendations, not re-proposed.
3. Run `search_tickets` per board and per signal (password reset, new user, access request, printer, VPN, license, etc.) rather than one giant query. If any search hits its result cap, say so and report that count as "at least N", never as exact.
4. Cluster tickets into candidate intents by the request the end user actually made, not by internal category.
5. Score each candidate: **volume** (tickets in window) × **automatability** (deterministic answer or scripted info-gathering = high; judgment, hands-on, or approval-heavy = low). Rank by the product.
6. For each of the top candidates, draft 8–12 varied trigger phrases mirroring how end users actually wrote them (sanitized — no names, clients, or systems specific to one tenant).
7. Output a ranked table: candidate intent, ticket count (with cap caveats), automatability rating and why, draft trigger phrases, and what the intent should collect or answer. Close by offering to hand the top pick to the Intent Builder skill.

## Guardrails

- Read-only. Never call `create_intent` from this skill — mining and building are separate confirmations.
- Skip any candidate that overlaps an existing intent's trigger phrases; list it under "already covered" instead.
- Volume estimates from capped searches must be labeled as floors, not totals.
- Do not propose intents for security-sensitive requests (password disclosure, MFA bypass) as self-service answers — flag them as identity-verification workflows instead.
- Sanitize all example phrasing: use <user>, <client>, <application> placeholders.
