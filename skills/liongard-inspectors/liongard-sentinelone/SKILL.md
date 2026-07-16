---
name: Liongard SentinelOne Read
description: Interrogate a client's SentinelOne tenant through the Liongard SentinelOne inspector — protected agents, agent/version health, threat detections, policy/site assignment, admins. Use for "are their S1 agents healthy?", coverage-gap checks, or EDR posture questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, liongard_alert, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard SentinelOne Read

Reads SentinelOne tenant state — enrolled agents, agent/version health, threat detections, site/policy assignment, and admins — from the Liongard SentinelOne inspector's dataprint, so a tech can judge EDR coverage without logging into the S1 console.

## When to use

- "Are <client>'s SentinelOne agents healthy and up to date?" / coverage-gap check.
- EDR/malware ticket needs the threat and health picture before triage.
- "Which endpoints aren't reporting to S1?" — coverage reconciliation.
- "Who has admin on their SentinelOne?" — access review.
- Post-incident: "did a policy or exclusion change on the S1 side?"

## What the inspector captures

Enrolled agents with health/online status, agent and threat-engine versions, threat/detection history, site and policy assignment, and tenant admins. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "SentinelOne", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Coverage: agent list with online/health status — count not-reporting / unhealthy.
   - Versions: agents on outdated agent/engine versions.
   - Threats: recent detections with mitigation status (resolved vs outstanding).
   - Admins: admin list with role.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to SentinelOne: policy changes, exclusion adds, admin adds/removes — line them up against the incident window. Use `liongard_alert` for anything Liongard already escalated.
4. Sanity-check surprising values (zero agents) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (unhealthy/not-reporting agents, outdated versions, outstanding threats, single-admin risk). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach coverage/health facts to a malware or EDR ticket.
- **Coverage reconciliation**: S1 agent count vs RMM/AD counts surfaces endpoints with no EDR — label the delta, verify before asserting.
- **QBR evidence**: EDR coverage %, agent-version currency, and outstanding-threat counts are recurring QBR line items — export with their as-of date.

## Guardrails

- Read-only: this skill never changes S1 policy, exclusions, or agent state. Remediation is a tech's job in the console.
- Absence of an agent in S1 is a *coverage lead*, not proof an endpoint is unprotected — verify before asserting.
- Always state dataprint age; agent health changes constantly — an old dataprint on an active incident deserves a "verify live" caveat.
- If no SentinelOne launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Admin-list findings are access-review inputs, not accusations — "unrecognized admin, verify" phrasing.
- Plain-text notes only.
