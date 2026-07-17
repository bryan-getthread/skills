---
name: Liongard Sophos Central Read
description: Interrogate a client's Sophos Central tenant through the Liongard Sophos Central inspector — protected endpoints, threat/health status, tamper protection, policy assignment, admin list. Use for "are their Sophos endpoints healthy?", coverage-gap checks during triage, or endpoint-security posture questions.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, liongard_alert]
connectors: [Liongard]
---

# Liongard Sophos Central Read

**When to use:** "Is <client> fully protected in Sophos?", an endpoint-security ticket needing the health/threat picture, "is tamper protection on across their fleet?", "who has admin on their Sophos Central?", or "did a policy or exclusion change?".

## Prompt

```
Read Sophos Central tenant state for CLIENT_NAME from the Liongard Sophos Central inspector. Read-only — Sophos policy, exclusions, and enrollment are a technician's job in the console.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Sophos" / "Sophos Central". Verify last-run success and note the dataprint age — carry "as of <timestamp>." Endpoint health changes constantly, so an old dataprint on an active incident deserves a "verify live" caveat.
2. Pick the angle and query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Coverage: endpoint list with protection status — count unprotected / not-reporting.
   - Health: devices where health/compliance != good — list for follow-up.
   - Tamper protection: devices with tamper protection disabled — hardening gap.
   - Threats: recent threat/alert entries with status (cleaned vs outstanding).
   - Admins: admin list with role.
3. "What changed?" → liongard_detection / liongard_timeline scoped to Sophos: policy changes, exclusion adds, admin adds/removes, agent-version shifts — ordered against the incident window. Use liongard_alert for anything already escalated.
4. Sanity-check surprising zeros (zero endpoints, no admins) — usually a wrong field path or partial inspection; re-probe broadly. Absence of an endpoint in Sophos is a coverage lead, not proof a machine is unprotected — a device may report to a different tenant; reconcile against RMM/AD counts and verify. Admin-list findings are review inputs — "unrecognized admin, verify" phrasing.
5. Output: a compact table, source + dataprint age line, flags (unprotected devices, tamper-off, outstanding threats, single-admin risk). Offer a plain-text add_ticket_note. Degradation: no Sophos Central launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify in console."
```
