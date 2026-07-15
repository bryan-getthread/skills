---
name: SOP Builder
description: Write a full standard operating procedure — scope, prerequisites, numbered procedure, validation, and escalation criteria — from a ticket, an existing rough doc, or a described process.
category: Documentation
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu]
---

# SOP Builder

Produce a complete, review-ready SOP document with every section a service desk needs to
run the procedure unattended by its author. Output is a draft — a human approves and files it.

## When to use

- "Write an SOP for new-user setup / backup verification / <procedure>."
- A ticket resolution needs to become a repeatable procedure, not just a KB article.
- A rough checklist or tribal-knowledge process needs formalizing.
- sop-candidate-finder flagged a resolution worth proceduralizing.

## Steps

1. Gather the source material: the ticket thread via `search_tickets`, any existing partial docs via `search_knowledge_base` / `search_itglue` / `search_hudu` (where enabled), and anything the requester pastes. If an SOP already exists for this procedure, propose a revision instead of a duplicate.
2. Draft the SOP in this fixed structure:
   - **Title & purpose** — what this procedure accomplishes, in one sentence.
   - **Scope** — which clients/environments/situations it covers, and explicitly what it does NOT cover.
   - **Prerequisites** — required access/roles, tools, portals, approvals, and information to collect before starting.
   - **Procedure** — numbered, imperative steps. One action per step. Include expected result after any step where success is observable. Keep branches inline ("If X → step 12; otherwise continue").
   - **Validation** — how to prove the procedure worked: specific checks, what output/state to verify, and what to record in the ticket.
   - **Escalate when** — concrete conditions that mean stop and escalate (errors encountered, time-box exceeded, out-of-scope discovery), and to which tier/queue.
   - **Revision block** — placeholder for owner, date, and review cadence.
3. Sanitize: placeholders for client names, hostnames, IPs, and tenants (`<client>`, `<server>`, `<tenant>`); strip credentials entirely and reference the credential store instead ("retrieve admin credentials from the documentation platform").
4. Pressure-test the draft: could a competent tech who has never done this follow it? Flag steps that assume unstated knowledge with `[verify: ...]` markers for the reviewer.
5. Output the SOP in chat for review. Do not file it into any documentation platform yourself.

## Guardrails

- Draft only — never auto-publish or write into IT Glue/Hudu/the KB.
- Base the procedure on evidence (thread, docs, requester statements). Where you must fill a gap, mark it `[verify: ...]` rather than presenting a guess as fact.
- Never embed credentials, MFA secrets, or license keys — reference the credential store.
- The "Escalate when" section is mandatory; an SOP without a stop condition is incomplete.
- Keep language plain and localizable — no idioms; the SOP may run on a non-English desk.
