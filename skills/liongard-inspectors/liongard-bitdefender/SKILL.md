---
name: Liongard Bitdefender Read
description: Interrogate a client's Bitdefender GravityZone tenant through the Liongard Bitdefender inspector — protected endpoints, agent/module status, threat detections, policy assignment, admins. Use for "are their Bitdefender endpoints protected?", coverage-gap checks, or AV/EDR posture questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, liongard_alert, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Bitdefender Read

Reads Bitdefender GravityZone state — enrolled endpoints, agent/module status, threat detections, policy assignment, and admins — from the Liongard Bitdefender inspector's dataprint, so a tech can judge endpoint-security coverage without logging into the GravityZone console.

## When to use

- "Are <client>'s Bitdefender endpoints protected and reporting?" / coverage-gap check.
- Malware/AV ticket needs the health and threat picture before triage.
- "Which endpoints have protection modules disabled?" — hardening question.
- "Who has admin on their GravityZone?" — access review.
- Post-incident: "did a policy or exclusion change on the Bitdefender side?"

## What the inspector captures

Enrolled endpoints with protection/online status, installed module state, agent versions, threat/detection history, policy assignment, and tenant admins. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Bitdefender" / "GravityZone", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Coverage: endpoint list with protection status — count unprotected / not-reporting.
   - Modules: endpoints with a protection module disabled.
   - Threats: recent detections with status (resolved vs outstanding).
   - Admins: admin list with role.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to Bitdefender: policy changes, exclusion adds, admin adds/removes — line them up against the incident window. Use `liongard_alert` for escalated items.
4. Sanity-check surprising values (zero endpoints) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (unprotected endpoints, disabled modules, outstanding threats, single-admin risk). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach coverage/health facts to a malware or endpoint ticket.
- **Coverage reconciliation**: Bitdefender endpoint count vs RMM/AD counts surfaces machines with no AV — label the delta, verify before asserting.
- **QBR evidence**: protection coverage %, module adoption, and outstanding-threat counts are recurring QBR line items — export with their as-of date.

## Guardrails

- Read-only: this skill never changes Bitdefender policy, exclusions, or enrollment. Remediation is a tech's job in the console.
- Absence of an endpoint in Bitdefender is a *coverage lead*, not proof a machine is unprotected — verify before asserting.
- Always state dataprint age; endpoint health changes constantly — an old dataprint on an active incident deserves a "verify live" caveat.
- If no Bitdefender launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Admin-list findings are access-review inputs, not accusations — "unrecognized admin, verify" phrasing.
- Plain-text notes only.
