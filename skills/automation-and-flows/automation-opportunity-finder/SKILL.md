---
name: Automation Opportunity Finder
description: Mine recent tickets for repetitive manual work, estimate the time each pattern burns, and route every finding to the right fix — flow, intent, RMM policy, monitoring change, or training. Use when asked "what should we automate", "find our zero-touch opportunities", or "where is the team wasting time".
category: Automation & Flows
tools: [search_tickets, list_boards, list_flows, list_intents]
---

# Automation Opportunity Finder

A wider net than Intent Mining: find *all* repetitive manual work in the ticket stream — not just deflectable requests — estimate the hours it consumes, and route each pattern to the mechanism that actually kills it.

## When to use

- "Analyze last quarter's tickets and tell me what to automate."
- "We want to raise zero-touch resolution — where's the low-hanging fruit?"
- An exec asks for an automation ROI story with numbers behind it.

## Steps

1. Confirm the window (default 90 days) and boards (`list_boards`). Run `search_tickets` per board and per recurrence signal (same title shape, same alert source, same request type) — split searches; label capped counts as "at least N".
2. Cluster into recurring patterns. For each, capture: occurrences in window, whether resolution steps are identical each time, and who does the work.
3. **Estimate time savings** per pattern: occurrences × average handle time. Take handle time from time entries where they exist; otherwise use a stated assumption (e.g., "assuming 15 min each") and label it as an assumption, never as measured fact.
4. **Route each pattern** to its correct mechanism:
   - Predictable end-user request with a scriptable answer → **intent** (hand to Intent Builder).
   - Internal choreography on ticket events (statuses, assignments, notes, notifications) → **flow** (hand to Flow Builder).
   - Same technical fix on endpoints (disk cleanup, service restart, patching) → **RMM script/policy** — note the tech executes this in the RMM; Super Magic can deep-link a device but cannot run scripts.
   - Alert noise with no action ever taken → **monitoring tuning** (fix the sensor, not the ticket).
   - Same how-do-I question from staff or repeated tech mistakes → **training or KB article**.
5. Check `list_intents` and `list_flows`; anything already covered moves to an "already automated — verify it's working" list instead of the build list.
6. Output a ranked table: pattern, occurrences (with cap caveats), estimated hours/month (with assumptions), routed mechanism, and a one-line next step. Rank by estimated hours saved × ease of implementation.

## Guardrails

- Read-only analysis; never create flows or intents from this skill — hand off to the builder skills, each with its own confirmation gate.
- Time-savings figures built on assumed handle times must say so in the same sentence as the number.
- Don't recommend automating judgment-heavy or safety-relevant work (security response, offboarding approvals) end-to-end; route those to "assisted" rather than "zero-touch".
- Sanitize examples: <client>, <vendor>, <device> placeholders.
