---
name: Service Catalog Design
description: Mine the recurring request patterns out of the ticket history and turn them into a defined service catalog — request types with SLAs, fulfillment steps, and which ones are ripe for intent automation — so common requests stop being improvised every time.
category: Change & Problem Management
tools: [search_tickets, search_knowledge_base, add_ticket_note, list_boards, list_intents]
---

# Service Catalog Design

A service catalog is the difference between "email us and we'll figure it out" and a defined menu: this request type, this information required, these steps, this turnaround. This skill builds catalog entries from what the desk actually receives — evidence-first, not aspirational — and flags the entries that should become automated intents.

## When to use

- "What should be in our service catalog?" / "define our standard request types."
- A recurring request keeps being handled inconsistently (different techs, different steps, different turnarounds).
- Preparing intent/automation candidates: which request types are defined enough to automate?
- Refreshing an existing catalog against what the desk actually receives now.

## Steps

1. **Mine the patterns**: search_tickets over a representative window (default 90 days) for request-type tickets — split searches per board and per signal to respect result caps, and state when counts may be capped. Cluster by what was actually asked: new-user setup, offboarding, password/MFA reset, software install, access/permission grant, mailbox or distribution-list change, new device provisioning, guest access, file restore, and the desk's own recurring long tail.
2. Rank clusters by frequency and by inconsistency cost (same request, wildly varying handling time or steps = the highest-value entries to define). Present the ranked list before drafting entries — the requester picks what makes the catalog cut; not everything recurring deserves an entry.
3. For each selected request type, draft the catalog entry from the evidence of how good handling actually looked in the tickets:
   - **Name and description** — in requester language, plus what this request type does NOT cover (scope edges prevent catalog abuse).
   - **Who may request it** — any user vs. named client approver required (access grants and offboarding almost always need the approver; capture the desk's actual verification practice, and see identity-verification norms in the security skills).
   - **Required information at intake** — the fields whose absence caused the round-trips visible in the ticket history. This list is what kills the back-and-forth.
   - **Fulfillment steps** — numbered, from the tickets where handling went well; reference existing SOPs/runbooks (search_knowledge_base) rather than duplicating them.
   - **Approval gates** — which steps need whose sign-off before proceeding (real gates: silence is not approval).
   - **Target turnaround** — proposed from observed handling times of well-run instances, not from wishful thinking; the formal SLA treatment lives in request-fulfillment-sla.
4. **Flag intent candidates**: entries with deterministic intake fields, low judgment in fulfillment, and high volume are automation-ripe. Check what already exists (list_intents where accessible) and hand the build itself to the desk's intent/flow authoring workflow (automation-and-flows category) — this skill nominates, it doesn't build.
5. Deliver the catalog draft in chat as the review artifact; on request, post entries into the desk's KB pipeline for publication (human-gated) and a plain-text summary note on the initiating ticket.
6. For a refresh: diff the existing catalog against the current 90-day pattern — entries with near-zero volume (retire candidates), high-volume requests with no entry (add candidates), and entries whose documented steps no longer match observed practice.

## Guardrails

- Evidence-first: every entry is grounded in observed tickets; the agent never invents demand or fabricates handling steps for requests the desk hasn't actually seen.
- The catalog defines requests, not incidents — something broken is an incident and stays out of the catalog (the Incident/Request split matters downstream for SLAs; see request-fulfillment-sla).
- Turnaround targets proposed here are proposals; committing them to clients is a human/contract decision.
- Result-cap honesty in the mining pass — a capped search understates volume, and the ranking must say so.
- Sanitized entries: request types and steps in general terms; no client names, no credentials, no environment-specific identifiers in the published catalog.
