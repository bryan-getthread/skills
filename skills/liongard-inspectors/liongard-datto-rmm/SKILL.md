---
name: Liongard Datto RMM Read
description: Interrogate a client's Datto RMM footprint through the Liongard Datto RMM inspector — managed device inventory, agent/online status, patch state, monitored alerts, sites. Use for "what devices does RMM manage for them?", agent-coverage gaps, or patch-currency checks during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard Datto RMM Read

**When to use:** "What devices does Datto RMM manage at <client>?", "which agents are offline / not checking in?", "are their machines patched?" during triage, reconciling RMM count against AD/security-tool counts, or "did monitoring or patch policy change?".

## Prompt

```
Read Datto RMM state for CLIENT_NAME from the Liongard Datto RMM inspector. Read-only — this never runs scripts, deploys software, or changes RMM policy.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Datto RMM" / "Datto". Verify last-run success and note the dataprint age — carry "as of <timestamp>." Agent status changes constantly, so an old dataprint on an active incident deserves a "verify live" caveat.
2. Pick the angle and query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Inventory: device list with OS and last-seen — group by site.
   - Offline agents: devices whose last-check-in exceeds a threshold.
   - Patch state: devices with missing/failed patches.
   - Alerts: open monitored alerts with severity.
3. "What changed?" → liongard_detection / liongard_timeline scoped to Datto RMM: device adds/removes, monitor/policy changes, agent-version shifts — ordered against the incident window.
4. Sanity-check surprising zeros (zero devices) — usually a wrong field path or partial inspection; re-probe broadly. Absence of a device in RMM is a coverage lead, not proof it is unmanaged — reconcile against AD/AV counts and verify before asserting.
5. Output: a compact table, source + dataprint age line, flags (offline agents, patch laggards, coverage gaps). Offer a plain-text add_ticket_note. Degradation: no Datto RMM launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify in console."
```
