---
name: Secure Score Improvement
description: A client wants their M365 Secure Score (or similar posture score) improved — build the phased plan that separates quick wins from user-impacting changes, and gate every impactful change behind client sign-off.
category: Security
tools: [search_tickets, search_clients, search_itglue, add_ticket_note, create_ticket, update_ticket, send_approval, view_openDraft]
---

# Secure Score Improvement

Posture scores move two ways: silent admin-side hardening, and changes users will feel Monday morning. This skill turns "get our score up" into a phased plan that ships the quick wins immediately and walks the impactful changes through client sign-off — because a score improved by surprise-blocking the CEO's mail rules is a score that gets reverted.

## When to use

- A client asks to improve their Secure Score / posture score, often after seeing it in a report or an insurance requirement
- A posture review (cyber-risk-posture-review) produced recommendations that now need a rollout plan
- Management wants score-improvement work scoped for a quote or project

## Steps

1. Start from the current recommendations export: work from the score dashboard's improvement-action list (provided by the tech or the client — record its as-of date). Do not invent actions from memory; the platform's list is the source of truth and it changes as the vendor adds controls.
2. Classify every improvement action on two axes:
   - Impact on users: none/invisible (audit settings, admin-side hardening) vs noticeable (MFA prompts, legacy-auth cutoff, external-mail tagging) vs disruptive-if-unplanned (blocking legacy protocols still in use, attachment-type blocks, mandatory device compliance).
   - Effort/risk: config-only vs needs-discovery-first (who still uses the thing being blocked?) vs licensing-dependent (some controls require higher tiers — flag cost implications for the account manager, don't promise them).
3. Discovery before cutoffs: for every "block/disable" action, check what would break — sign-in logs for legacy-auth usage, app inventories for the protocol in question (search_itglue for documented line-of-business dependencies; search_tickets for past incidents when similar changes were tried). A control that breaks the client's scanner-to-email workflow costs more trust than the points are worth.
4. Build the phased plan:
   - Phase 1 — quick wins: invisible, config-only actions. Batch them into a change ticket; standard change process applies.
   - Phase 2 — user-noticeable: bundle with communication (end-user notice drafted per the maintenance-window-notice / email-baseline-standard patterns), a pilot group where feasible, and a scheduled window.
   - Phase 3 — disruptive/dependency-laden: each gets its own discovery result, rollback plan, and explicit client sign-off.
   Sequence by points-per-disruption, not raw points.
5. Client sign-off gate: present the plan with, per phase, what changes, who notices, the score delta claimed by the platform, and the rollback. Send for approval (send_approval where available; otherwise a drafted email via view_openDraft for human send). No Phase 2/3 change ships without recorded client approval — verbal doesn't count.
6. Execute as change tickets per phase (create_ticket), technician executes in the admin console, agent tracks and records. After each phase: re-check the score, verify nothing broke (helpdesk volume watch for 48–72h post-change), and update the plan note. Report progress in the client's monthly-security-report.

## Guardrails

- Never ship user-impacting or dependency-risky changes without recorded client sign-off and a rollback plan; quick wins still go through the desk's normal change process.
- Score is a proxy, not the goal: never recommend gaming actions (e.g. marking items "resolved through third party" without a real third-party control in place) — that converts a posture gap into a documented false statement.
- Discovery before disabling: no legacy-protocol or feature cutoff without evidence of current usage checked first.
- Licensing-gated controls are flagged with their dependency, never silently included in promised score gains.
- The agent plans, tracks, and documents; the technician executes tenant changes.
- Degradation: without a current recommendations export the plan cannot be built — request it rather than working from a stale or remembered list.
