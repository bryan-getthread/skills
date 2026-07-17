---
name: Fleet Health Sweep
description: Sweep an entire client's device fleet through the RMM — offline devices, alert clusters, disk pressure, missing patches — and rank the top issues that need attention. Use for "which devices at <client> need attention" or a proactive per-client health pass.
category: Devices & Infrastructure
tools: [list_ninjaone_organizations, search_ninjaone_devices, get_ninjaone_device, list_ninjaone_alerts, add_ticket_note]
connectors: [NinjaOne]
---

# Fleet Health Sweep

**When to use:** "Which devices at <client> need attention?", a proactive weekly/monthly health pass, or prepping for an on-site visit / QBR.

## Prompt

```
Whole-client RMM sweep rolled up into a ranked top-N list of issues worth a tech's time. Read-only. Requires NinjaOne; if absent, say so (and if ConnectWise RMM is the enabled RMM instead, state the sweep should run against it and cover what its device reads expose).

1. Resolve the client to a single RMM organization via list_ninjaone_organizations. If names are ambiguous, rank by exact-name match and state your choice; do not stop to ask unless two organizations are genuinely indistinguishable.
2. Pull the device list with search_ninjaone_devices scoped to the organization, and list_ninjaone_alerts for the organization in parallel. Do not trust node_class filters — spot-check returned devices' class in details before splitting servers from workstations.
3. If either listing hits a result cap, report "at least N devices / alerts" and sweep in segments (per node class, per location) rather than presenting a truncated list as complete.
4. Bucket findings: (a) offline devices with days since last contact — separate long-dead (30+ days, likely retired) from recently dropped; (b) alert clusters — group by device and by type; a device with many alert types outranks many devices with one benign type; (c) disk pressure — system volumes under ~15% free or under 10 GB; (d) patch posture — devices with failed patches or clearly stale OS builds where details expose it.
5. Rank the top N (default 10) by blast radius and severity: servers before workstations, shared infrastructure before single-user, active alerts before stale.
6. Output: a short fleet summary (total devices, online/offline split, alert count), then the ranked issue list with device name, problem, and concrete next action. Offer to post the summary as a plain-text note via add_ticket_note (no markdown/emojis), or to run a deep single-device Device Health Check on any entry.

Guardrails: read-only — never reset alerts, reboot, or change anything during a sweep; propose actions, do not take them. Result-cap honesty is mandatory — a possibly-truncated fleet number is a floor, never exact. Long-offline devices may be retired hardware still enrolled — flag "verify still in service", not incidents. Use <client> in any templated example output — no real client names.

Note: Flows cannot schedule or time-trigger this — Flows fire on ticket events only. Run it manually on demand, or from an external scheduler; a Flow can only reach it via Run Skill on a qualifying ticket event.

Unattended (Flow) mode: entire reply is the plain-text fleet digest posted verbatim (summary counts, then ranked issue list with device, problem, next action). Input is the RMM organization id (never a name to resolve unattended); id missing or not found -> output nothing. Capped listings make every affected count "at least N" and the summary line gains "SWEEP PARTIAL". Permitted write: add_ticket_note to the designated destination only — no resets, reboots, or maintenance changes. NinjaOne not enabled -> output nothing.
```
