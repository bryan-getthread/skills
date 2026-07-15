---
name: Queue Scoring Triage
description: Produce a ranked triage order for the queue using two-layer scoring — a per-queue baseline plus per-ticket modifiers — so techs work the right ticket next.
category: Triage & Routing
tools: [search_tickets, list_boards, list_ticket_priorities, search_clients]
---

# Queue Scoring Triage

Rank the open queue with an explainable two-layer score: every board/queue carries a baseline weight, and each ticket earns modifiers on top (priority, age, SLA pressure, client tier, sentiment, waiting-on state). Output is a ranked list a dispatcher can act on — with the math shown.

## When to use

- "What order should we work the queue in?" / "what's next for the team?"
- A dispatcher wants a scored morning triage list across boards.
- A lead wants to sanity-check that high-priority work is actually surfacing first.

## Steps

1. Fetch open tickets with `search_tickets`, split per board (`list_boards`) to stay under result caps; disclose if any board's query may have capped.
2. Layer 1 — queue baseline: assign each board its configured baseline score (e.g. security board 40, NOC 30, help desk 20, internal 5). Unconfigured boards get the default baseline and are called out.
3. Layer 2 — ticket modifiers, additive and bounded, e.g.: priority level (from `list_ticket_priorities` mapping), age beyond target, approaching/breached SLA, VIP or strategic client (via `search_clients` tier data where present), negative sentiment, and a penalty (not a boost) for waiting-on-client/vendor states.
4. Compute score = baseline + modifiers for every ticket. Ties break by oldest-first.
5. Output the ranked list: rank, ticket number, client, one-line summary, total score, and the breakdown (baseline + each modifier). Flag anomalies — e.g. a P1 ranking below page one, or a whole board starved.

## Guardrails

- Read-only: this skill ranks and reports; it never changes priorities, statuses, or assignments. Acting on the ranking is a human (or separate skill) decision.
- Every score must be reproducible from the shown breakdown — no hidden judgment bumps. If you adjust a rank for a reason outside the model, label it explicitly as an override suggestion.
- Waiting-on-client/vendor tickets are de-ranked, not hidden — stalls are a separate follow-up workflow.
- If scoring weights are not configured for this desk, use the documented defaults and say so; do not silently invent weights per run (rankings must be stable between runs).
- Disclose result caps; a ranking over a truncated queue must say it is partial.
