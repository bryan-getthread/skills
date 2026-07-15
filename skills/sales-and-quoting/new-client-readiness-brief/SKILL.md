---
name: New Client Readiness Brief
description: When a prospect is close to signing and the service desk needs a day-one readiness brief — what must exist, be collected, and be configured before the first ticket arrives.
category: Sales & Quoting
tools: [search_clients, search_members, list_boards, search_knowledge_base, create_ticket]
---

# New Client Readiness Brief

Turn "they're about to sign" into a concrete day-one plan: everything the service desk needs collected, configured, and assigned before the new client's first ticket lands — so the first week feels like proof, not chaos.

## When to use

- "<prospect> signs Friday — what does the desk need for day one?"
- "Build the onboarding readiness brief for the new client."
- "Are we ready to support <prospect> from the moment the agreement starts?"

## Steps

1. Gather what's known: client size, sites, user count, key applications, and what was sold (which services, response targets) — from the requester and any pre-sales notes they can paste. What was sold defines what must be ready.
2. Check the desk's own standard: `search_knowledge_base` for an existing client-onboarding SOP or checklist. If one exists, use it as the spine and fill it; if not, use the structure below and flag that the desk should canonize one.
3. Build the brief in six sections, each item marked Ready / Needed / Owner-unassigned:
   - **Intake** — client record created (verify against `search_clients` to avoid duplicates), boards/routing decided (`list_boards`), how their users will reach the desk (email address, messenger, phone) communicated to whom on the client side.
   - **People** — assigned primary tech/pod and account owner (`search_members`), who covers after-hours if sold.
   - **Access & credentials** — admin credentials, domain/DNS, M365/tenant access, network gear, vendor accounts — the collect-securely list (into the desk's credential store, never tickets or email).
   - **Environment knowledge** — asset inventory source, documentation to be captured in week one, known problem areas disclosed during the sale.
   - **Tooling deployment** — RMM agent rollout plan, monitoring thresholds, backup verification, security stack per what was sold.
   - **Expectations** — response targets promised, escalation path, VIP contacts, first-30-days check-in schedule.
4. Sequence it: what must exist BEFORE go-live (intake, people, access) vs the first two weeks (full documentation, tooling saturation). Every "Needed" item gets a proposed owner.
5. Output the brief in chat. With the requester's confirmation, `create_ticket` for the onboarding project ticket carrying the checklist as the description (plain text), assigned to the onboarding owner.

## Guardrails

- Do not create the client record or any ticket until the requester confirms — the deal isn't done until it's done, and duplicate client records are painful.
- Never state what was sold or promised as fact without the source (the requester's word, a pasted proposal); mismatches between "sold" and "deliverable day one" are flagged, not smoothed over.
- Credentials are collected into the desk's credential-management practice — the brief says WHAT to collect, never becomes a place credentials are written down.
- Response/SLA expectations in the brief come from the agreement evidence; if unavailable, mark them "confirm from signed agreement" rather than guessing.
- Internal document; nothing here goes to the client except through the account owner's kickoff communication.
