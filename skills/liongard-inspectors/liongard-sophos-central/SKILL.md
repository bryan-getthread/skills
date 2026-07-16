---
name: Liongard Sophos Central Read
description: Interrogate a client's Sophos Central tenant through the Liongard Sophos Central inspector — protected endpoints, threat/health status, tamper protection, policy assignment, admin list. Use for "are their Sophos endpoints healthy?", coverage-gap checks during triage, or endpoint-security posture questions.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, liongard_alert, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Sophos Central Read

Reads Sophos Central tenant state — enrolled endpoints, protection/health status, tamper protection, threat detections, policy assignments, and admins — from the Liongard Sophos Central inspector's dataprint, so a tech can judge endpoint-security coverage without logging into the Sophos console.

## When to use

- "Is <client> fully protected in Sophos?" / coverage-gap check (devices seen elsewhere but missing from Sophos).
- Endpoint-security ticket needs the health/threat picture before triage.
- "Is tamper protection on across their fleet?" — hardening or audit question.
- "Who has admin on their Sophos Central?" — access review or offboarding.
- Post-incident: "did a policy or exclusion change on the Sophos side?"

## What the inspector captures

Enrolled devices with protection status, health/compliance state, tamper-protection flag, product/agent versions, threat and alert history, policy assignments, and tenant admins. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Sophos" / "Sophos Central", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Coverage: endpoint list with protection status — count unprotected / not-reporting devices.
   - Health: devices where health/compliance != good — list for follow-up.
   - Tamper protection: devices with tamper protection disabled — hardening gap.
   - Threats: recent threat/alert entries with status (cleaned vs outstanding).
   - Admins: admin list with role — count and name for access review.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to Sophos: policy changes, exclusion adds, admin adds/removes, agent-version shifts — line them up against the incident window. Use `liongard_alert` for anything Liongard already escalated.
4. Sanity-check surprising values (zero endpoints, no admins) — usually a wrong field path or partial inspection, not reality. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (unprotected devices, tamper-off, outstanding threats, single-admin risk). Offer a plain-text `add_ticket_note` for the active ticket.

## How this feeds tickets

- **Triage**: attach coverage/health facts to a malware or endpoint ticket so the tech starts with the security posture.
- **Coverage reconciliation**: cross-reference Sophos endpoint count against RMM/AD device counts to surface machines with no AV — label the delta, don't assume cause.
- **QBR evidence**: protection coverage %, tamper-protection adoption, and outstanding-threat counts are recurring QBR line items — export with their as-of date.

## Guardrails

- Read-only: this skill never changes Sophos policy, exclusions, or enrollment. Remediation is a tech's job via the Sophos console.
- Absence of an endpoint in Sophos is a *coverage lead*, not proof a machine is unprotected — a device may report to a different tenant. Verify before asserting.
- Always state dataprint age; endpoint health changes constantly, so an old dataprint on an active incident deserves a "may be stale — verify live" caveat.
- If no Sophos Central launchpoint exists, degrade per the access pattern: docs (`search_itglue`/`search_hudu`) → ticket history → "verify in console".
- Admin-list findings are access-review inputs, not accusations — "unrecognized admin, verify" phrasing.
- Plain-text notes only.
