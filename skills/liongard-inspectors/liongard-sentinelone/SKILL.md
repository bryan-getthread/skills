---
name: Liongard SentinelOne Read
description: Interrogate a client's SentinelOne tenant through the Liongard SentinelOne inspector — protected agents, agent/version health, threat detections, policy/site assignment, admins. Use for "are their S1 agents healthy?", coverage-gap checks, or EDR posture questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, liongard_alert]
connectors: [Liongard]
---

# Liongard SentinelOne Read

**When to use:** "Are <client>'s SentinelOne agents healthy and up to date?", an EDR/malware ticket needing the threat picture, "which endpoints aren't reporting to S1?", "who has admin?", or "did a policy or exclusion change on the S1 side?".

## Prompt

```
Read SentinelOne tenant state for CLIENT_NAME from the Liongard SentinelOne inspector. Read-only — S1 policy, exclusions, and agent state are a technician's job in the console.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "SentinelOne". Verify last-run success and note the dataprint age — carry "as of <timestamp>." Endpoint health changes constantly, so an old dataprint on an active incident deserves a "verify live" caveat.
2. Pick the angle and query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Coverage: agent list with online/health status — count not-reporting / unhealthy.
   - Versions: agents on outdated agent/engine versions.
   - Threats: recent detections with mitigation status (resolved vs outstanding).
   - Admins: admin list with role.
3. "What changed?" → liongard_detection / liongard_timeline scoped to SentinelOne: policy changes, exclusion adds, admin adds/removes — ordered against the incident window. Use liongard_alert for anything Liongard already escalated.
4. Sanity-check surprising zeros (zero agents) — usually a wrong field path or partial inspection; re-probe broadly. Absence of an agent in S1 is a coverage lead, not proof an endpoint is unprotected — reconcile against RMM/AD counts and verify before asserting. Admin-list findings are review inputs — "unrecognized admin, verify" phrasing.
5. Output: a compact table, source + dataprint age line, flags (unhealthy/not-reporting agents, outdated versions, outstanding threats, single-admin risk). Offer a plain-text add_ticket_note. Degradation: no SentinelOne launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify in console."
```
