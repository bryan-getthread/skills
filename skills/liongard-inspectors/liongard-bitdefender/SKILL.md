---
name: Liongard Bitdefender Read
description: Interrogate a client's Bitdefender GravityZone tenant through the Liongard Bitdefender inspector — protected endpoints, agent/module status, threat detections, policy assignment, admins. Use for "are their Bitdefender endpoints protected?", coverage-gap checks, or AV/EDR posture questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, liongard_alert]
connectors: [Liongard]
---

# Liongard Bitdefender Read

**When to use:** "Are <client>'s Bitdefender endpoints protected and reporting?", a malware/AV ticket needing the health picture, "which endpoints have protection modules disabled?", "who has admin on their GravityZone?", or "did a policy or exclusion change?".

## Prompt

```
Read Bitdefender GravityZone state for CLIENT_NAME from the Liongard Bitdefender inspector. Read-only — Bitdefender policy, exclusions, and enrollment are a technician's job in the console.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Bitdefender" / "GravityZone". Verify last-run success and note the dataprint age — carry "as of <timestamp>." Endpoint health changes constantly, so an old dataprint on an active incident deserves a "verify live" caveat.
2. Pick the angle and query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Coverage: endpoint list with protection status — count unprotected / not-reporting.
   - Modules: endpoints with a protection module disabled.
   - Threats: recent detections with status (resolved vs outstanding).
   - Admins: admin list with role.
3. "What changed?" → liongard_detection / liongard_timeline scoped to Bitdefender: policy changes, exclusion adds, admin adds/removes — ordered against the incident window. Use liongard_alert for escalated items.
4. Sanity-check surprising zeros (zero endpoints) — usually a wrong field path or partial inspection; re-probe broadly. Absence of an endpoint in Bitdefender is a coverage lead, not proof a machine is unprotected — reconcile against RMM/AD counts and verify before asserting. Admin-list findings are review inputs — "unrecognized admin, verify" phrasing.
5. Output: a compact table, source + dataprint age line, flags (unprotected endpoints, disabled modules, outstanding threats, single-admin risk). Offer a plain-text add_ticket_note. Degradation: no Bitdefender launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify in console."
```
