---
name: Liongard ConnectWise Automate Read
description: Interrogate a client's ConnectWise Automate (LabTech) footprint through the Liongard Automate inspector — managed computer inventory, agent check-in status, patch state, monitors, locations. Use for "what does Automate manage for them?", offline-agent checks, or patch-currency questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard ConnectWise Automate Read

**When to use:** "What computers does Automate manage at <client>?", "which agents haven't checked in?", "are machines patched?" during triage, reconciling Automate count against AD/security-tool counts, or "did a monitor or patch policy change?".

## Prompt

```
Read ConnectWise Automate (LabTech) state for CLIENT_NAME from the Liongard Automate inspector. Read-only — this never runs scripts, deploys software, or changes Automate policy.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Automate" / "ConnectWise Automate" / "LabTech". Verify last-run success and note the dataprint age — carry "as of <timestamp>." Agent status changes constantly, so an old dataprint on an active incident deserves a "verify live" caveat.
2. Pick the angle and query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Inventory: computer list with OS and last-contact — group by location.
   - Offline agents: computers whose last-contact exceeds a threshold.
   - Patch state: machines with missing/failed patches.
   - Monitors: monitors in a failed/alerting state.
3. "What changed?" → liongard_detection / liongard_timeline scoped to Automate: agent adds/removes, monitor/policy changes — ordered against the incident window.
4. Sanity-check surprising zeros (zero computers) — usually a wrong field path or partial inspection; re-probe broadly. Absence of an agent in Automate is a coverage lead, not proof a machine is unmanaged — reconcile against AD/AV counts and verify before asserting.
5. Output: a compact table, source + dataprint age line, flags (offline agents, patch laggards, coverage gaps). Offer a plain-text add_ticket_note. Degradation: no Automate launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify in console."
```
