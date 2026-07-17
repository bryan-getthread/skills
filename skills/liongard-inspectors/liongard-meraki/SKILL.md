---
name: Liongard Meraki Read
description: Interrogate a client's Cisco Meraki org through the Liongard Meraki inspector — SSIDs, VLANs, firmware, admin list, device inventory, license state. Use for "what's on their Meraki", wireless/VLAN config questions during triage, or license-expiry checks.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard Meraki Read

**When to use:** "What SSIDs are broadcast / which VLAN is the guest wifi on at <client>?", a wifi or connectivity ticket, "who has admin on their Meraki org?", "when do their Meraki licenses expire / what firmware are the APs on?", or "did anything change on the Meraki side?".

## Prompt

```
Read Meraki dashboard state for CLIENT_NAME from the Liongard Cisco Meraki inspector. Read-only — Meraki changes go through the partner's change process.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Meraki" (a client may have one org-level launchpoint covering many networks). Verify last-run success and note the dataprint age — carry "as of <timestamp>."
2. Pick the angle and query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - SSIDs: enabled SSIDs with auth mode and VLAN tagging (angle to verify: `Networks[].SSIDs[?Enabled==\`true\`].{Name:Name, Auth:AuthMode, VLAN:VlanId}`).
   - VLANs: subnet map (angle to verify: `Networks[].VLANs[].{Id:Id, Name:Name, Subnet:Subnet}`).
   - Firmware: device list grouped by model + firmware — flag mixed versions within a model family.
   - Admins: org admin list with privilege ("full" vs "read-only") — count full admins and list names.
   - Licensing: license state and expiration — flag anything expiring within 90 days.
3. "What changed?" → liongard_detection / liongard_timeline scoped to Meraki: SSID/PSK changes, VLAN edits, admin adds/removes, firmware upgrades — ordered against the incident window (time-adjacency, not proven cause). Wireless config changes often, so an old dataprint on an active wifi incident deserves a "may be stale — verify live" caveat.
4. Sanity-check surprising zeros (zero SSIDs, no admins) — usually a wrong field path or partial inspection; re-probe broadly. Never paste PSKs or credentials into notes — reference "PSK on file in <doc platform>." Admin-list findings are review inputs — "unrecognized admin, verify" phrasing.
5. Output: a compact table, source + dataprint age line, flags (expiring licenses, single-admin risk, firmware drift, unexplained recent changes). Offer a plain-text add_ticket_note. Degradation: no Meraki launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify on dashboard."
```
