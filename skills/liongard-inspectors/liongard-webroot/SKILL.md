---
name: Liongard Webroot Read
description: Interrogate a client's Webroot (GSM) console through the Liongard Webroot inspector — protected endpoints, agent status, infection/threat state, policy assignment, sites. Use for "are their Webroot endpoints protected?", coverage-gap checks, or AV posture questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, liongard_alert, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Webroot Read

Reads Webroot (Global Site Manager) state — protected endpoints, agent status, infection/threat state, policy assignment, and site structure — from the Liongard Webroot inspector's dataprint, so a tech can judge AV coverage without logging into the Webroot console.

## When to use

- "Are <client>'s Webroot endpoints protected and reporting?" / coverage-gap check.
- Malware/AV ticket needs the endpoint and infection picture before triage.
- "Which endpoints are flagged as infected or needing attention?"
- "Which sites/policies is a device assigned to?"
- Post-change: "did a policy assignment change on the Webroot side?"

## What the inspector captures

Enrolled endpoints with protection/online status, agent version, infection/threat state, policy assignment, and site/console structure. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Webroot", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Coverage: endpoint list with protection status — count unprotected / not-reporting.
   - Infections: endpoints flagged infected or needing attention.
   - Policy: policy/site assignment per device.
   - Versions: agents on outdated versions.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to Webroot: policy changes, endpoint adds/removes — line them up against the incident window. Use `liongard_alert` for escalated items.
4. Sanity-check surprising values (zero endpoints) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (unprotected endpoints, infected devices, coverage gaps). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach coverage/infection facts to a malware or endpoint ticket.
- **Coverage reconciliation**: Webroot endpoint count vs RMM/AD counts surfaces machines with no AV — label the delta, verify before asserting.
- **QBR evidence**: protection coverage % and infection counts are recurring QBR line items — export with their as-of date.

## Guardrails

- Read-only: this skill never changes Webroot policy or enrollment. Remediation is a tech's job in the console.
- Absence of an endpoint in Webroot is a *coverage lead*, not proof a machine is unprotected — verify before asserting.
- Always state dataprint age; endpoint state changes — an old dataprint on an active incident deserves a "verify live" caveat.
- If no Webroot launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Plain-text notes only.
