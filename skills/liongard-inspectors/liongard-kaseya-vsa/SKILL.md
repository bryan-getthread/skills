---
name: Liongard Kaseya VSA Read
description: Interrogate a client's Kaseya VSA footprint through the Liongard Kaseya VSA inspector — managed agent inventory, online/check-in status, patch state, monitor sets, organizations/machine groups. Use for "what does VSA manage for them?", offline-agent checks, or patch-currency questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard Kaseya VSA Read

**When to use:** "What machines does Kaseya VSA manage at <client>?", "which agents haven't checked in?", "are machines patched?" during triage, reconciling VSA count against AD/security-tool counts, or "did a monitor set or patch policy change?".

## Prompt

```
Read Kaseya VSA state for CLIENT_NAME from the Liongard Kaseya VSA inspector. Read-only — this never runs scripts, deploys software, or changes VSA policy.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Kaseya VSA" / "Kaseya" / "VSA". Verify last-run success and note the dataprint age — carry "as of <timestamp>." Agent status changes constantly, so an old dataprint on an active incident deserves a "verify live" caveat.
2. Pick the angle and query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Inventory: agent list with OS and last-check-in — group by machine group.
   - Offline agents: agents whose last-check-in exceeds a threshold.
   - Patch state: machines with missing/failed patches.
   - Monitors: monitor sets in an alarm state.
3. "What changed?" → liongard_detection / liongard_timeline scoped to VSA: agent adds/removes, monitor/policy changes — ordered against the incident window.
4. Sanity-check surprising zeros (zero agents) — usually a wrong field path or partial inspection; re-probe broadly. Absence of an agent in VSA is a coverage lead, not proof a machine is unmanaged — reconcile against AD/AV counts and verify before asserting.
5. Output: a compact table, source + dataprint age line, flags (offline agents, patch laggards, coverage gaps). Offer a plain-text add_ticket_note. Degradation: no Kaseya VSA launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify in console."
```
