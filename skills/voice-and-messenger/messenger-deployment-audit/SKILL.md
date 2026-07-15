---
name: Messenger Deployment Audit
description: Report which clients actually use Messenger versus which are entitled to it — deployed/active vs. silent — and surface the adoption gaps worth a rollout conversation. Use for "who has Messenger deployed", "which clients never chat", or QBR adoption prep.
category: Voice & Messenger
tools: [search_clients, search_tickets, list_boards]
---

# Messenger Deployment Audit

Every client without Messenger deployed is still emailing and calling — the expensive channels. This audit maps entitled vs. deployed vs. actually-used from observable chat activity, and hands back a gap list with the evidence behind each classification.

## When to use

- "Which of our clients have Messenger deployed and active?"
- "Who's entitled to Messenger but never uses it?" (adoption-gap hunting)
- QBR prep: one client's channel adoption story (pairs with Channel Volume Report for the volume side).
- After a Messenger rollout push: did the deployments stick?

## Steps

1. Enumerate active clients with `search_clients`. If the tenant's client records expose a Messenger/deployment status field, use it as the entitlement/deployment baseline; if not, say the baseline is inferred (step 3) — do not present inference as configuration.
2. Measure actual usage: `search_tickets` for chat/Messenger-sourced tickets per client over the window (default 90 days). Split searches per client or board where volumes are high, and state result-cap exposure — an undercount here mislabels a client as silent.
3. Classify each client:
   - **Active** — chat-sourced tickets in the window,
   - **Deployed but silent** — deployment status says enabled (or historical chat tickets exist) but zero chats in the window,
   - **Not deployed / unknown** — no deployment signal and no chat history ever.
4. For deployed-but-silent clients, pull one distinguishing fact where cheap: did they chat in a prior window (usage decayed — different conversation than never-adopted)?
5. Output the report: counts by classification, then the gap list — client, classification, evidence (last chat date or "none found"), and the suggested motion (rollout for never-deployed; re-introduction or champion outreach for decayed). Order by client size/ticket volume so the biggest deflection wins come first.
6. Note the caveat block: window used, result-cap exposure, and whether deployment status was read from records or inferred from activity.

## Guardrails

- Inference honesty is the whole product: "no chat tickets found" is evidence of non-usage, not proof of non-deployment — a widget can be installed and unused. Label every classification with its evidence type.
- Entitlement (what the client pays for) may not be visible from ticket data at all; if the tenant tracks it outside Thread, say the entitlement column needs that source rather than guessing.
- Read-only skill: no client records are modified, no outreach is sent. The output feeds a human's rollout plan.
- Zero-chat clients may be zero-chat by choice (a client that runs everything through one coordinator's email) — the report recommends conversations, not conclusions about client health.
- Sanitize for any exec-facing export: client names are fine internally; no contact-level naming needed anywhere in this report.
