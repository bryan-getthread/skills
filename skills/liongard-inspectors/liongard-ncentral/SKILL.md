---
name: Liongard N-central Read
description: Interrogate a client's N-able N-central footprint through the Liongard N-central inspector — managed device inventory, agent/probe status, patch state, monitored services, customers/sites. Use for "what does N-central manage for them?", offline-agent checks, or patch-currency questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard N-central Read

**When to use:** "What devices does N-central manage at <client>?", "which agents or probes are offline?", "are machines patched?" during triage, reconciling N-central count against AD/security-tool counts, or "did a monitoring service or patch policy change?".

## Prompt

```
Read N-able N-central state for CLIENT_NAME from the Liongard N-central inspector. Read-only — this never runs scripts, deploys software, or changes N-central policy.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "N-central" / "N-able". Verify last-run success and note the dataprint age — carry "as of <timestamp>." Agent status changes constantly, so an old dataprint on an active incident deserves a "verify live" caveat.
2. Pick the angle and query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Inventory: device list with OS and last-seen — group by customer/site.
   - Offline agents: devices whose last-check-in exceeds a threshold.
   - Patch state: machines with missing/failed patches.
   - Services: monitored services in a failed/warning state.
3. "What changed?" → liongard_detection / liongard_timeline scoped to N-central: device adds/removes, service/policy changes — ordered against the incident window.
4. Sanity-check surprising zeros (zero devices) — usually a wrong field path or partial inspection; re-probe broadly. Absence of a device in N-central is a coverage lead, not proof it is unmanaged — reconcile against AD/AV counts and verify before asserting.
5. Output: a compact table, source + dataprint age line, flags (offline agents, patch laggards, coverage gaps). Offer a plain-text add_ticket_note. Degradation: no N-central launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify in console."
```
