---
name: Fleet Health Sweep
description: Sweep an entire client's device fleet through the RMM — offline devices, alert clusters, disk pressure, missing patches — and rank the top issues that need attention. Use for "which devices at <client> need attention" or a proactive per-client health pass.
category: Devices & Infrastructure
tools: [list_ninjaone_organizations, search_ninjaone_devices, get_ninjaone_device, list_ninjaone_alerts, add_ticket_note]
---

# Fleet Health Sweep

Whole-client sweep of the RMM: every device's status, active alerts, disk pressure, and patch posture, rolled up into a ranked top-N list of issues worth a tech's time.

## When to use

- "Which devices at <client> need attention?"
- Proactive weekly/monthly health pass on a managed client before it turns into tickets.
- Prepping for an on-site visit or a QBR and wanting the fleet's problem list.
- "How many machines at <client> are offline / low on disk / behind on patches?"

## Steps

1. Resolve the client to a single RMM organization via `list_ninjaone_organizations`. If names are ambiguous, rank by exact-name match and state your choice; do not stop to ask unless two organizations are genuinely indistinguishable.
2. Pull the device list with `search_ninjaone_devices` scoped to the organization, and `list_ninjaone_alerts` for the organization in parallel. Do not trust node_class filters — spot-check returned devices' details for class correctness before splitting servers from workstations.
3. If either listing hits a result cap, note it: report "at least N devices / alerts" and sweep in segments (per node class, per location) rather than presenting a truncated list as complete.
4. Bucket findings: (a) offline devices, with days since last contact — separate long-dead (30+ days, likely retired or off-network) from recently dropped; (b) alert clusters — group alerts by device and by alert type; a device with many alert types outranks many devices with one benign type; (c) disk pressure — system volumes under ~15% free or under 10 GB; (d) patch posture — devices with failed patches or clearly stale OS builds, where the device details expose it.
5. Rank the top N issues (default 10) by blast radius and severity: servers before workstations, shared infrastructure before single-user machines, active alerts before stale ones.
6. Output: a short fleet summary (total devices, online/offline split, alert count), then the ranked issue list with device name, problem, and the concrete next action for each. Offer to post the summary as a plain-text note via `add_ticket_note`, or to run a deep single-device diagnosis (Device Health Check) on any entry.

## Guardrails

- This skill is read-only. Never reset alerts, reboot, or change anything during a sweep — propose actions, do not take them.
- Result-cap honesty is mandatory: a fleet number that might be truncated is presented as a floor, never an exact count.
- Long-offline devices may be retired hardware still enrolled in the RMM — flag them as "verify still in service", not as incidents.
- If NinjaOne is not enabled for the tenant, say so; do not substitute guesses. If ConnectWise RMM is the enabled RMM instead, state that this sweep should run against it and cover what its device reads expose.
- No client names in any example output you template; use <client>.
