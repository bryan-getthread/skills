---
name: Liongard Webroot Read
description: Interrogate a client's Webroot (GSM) console through the Liongard Webroot inspector — protected endpoints, agent status, infection/threat state, policy assignment, sites. Use for "are their Webroot endpoints protected?", coverage-gap checks, or AV posture questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, liongard_alert]
connectors: [Liongard]
---

# Liongard Webroot Read

**When to use:** "Are <client>'s Webroot endpoints protected and reporting?", a malware/AV ticket needing the infection picture, "which endpoints are flagged infected or needing attention?", "which sites/policies is a device assigned to?", or "did a policy assignment change?".

## Prompt

```
Read Webroot (Global Site Manager) state for CLIENT_NAME from the Liongard Webroot inspector. Read-only — Webroot policy and enrollment are a technician's job in the console.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Webroot". Verify last-run success and note the dataprint age — carry "as of <timestamp>." Endpoint state changes, so an old dataprint on an active incident deserves a "verify live" caveat.
2. Pick the angle and query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Coverage: endpoint list with protection status — count unprotected / not-reporting.
   - Infections: endpoints flagged infected or needing attention.
   - Policy: policy/site assignment per device.
   - Versions: agents on outdated versions.
3. "What changed?" → liongard_detection / liongard_timeline scoped to Webroot: policy changes, endpoint adds/removes — ordered against the incident window. Use liongard_alert for escalated items.
4. Sanity-check surprising zeros (zero endpoints) — usually a wrong field path or partial inspection; re-probe broadly. Absence of an endpoint in Webroot is a coverage lead, not proof a machine is unprotected — reconcile against RMM/AD counts and verify before asserting.
5. Output: a compact table, source + dataprint age line, flags (unprotected endpoints, infected devices, coverage gaps). Offer a plain-text add_ticket_note. Degradation: no Webroot launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify in console."
```
