---
name: Liongard Meraki Read
description: Interrogate a client's Cisco Meraki org through the Liongard Meraki inspector — SSIDs, VLANs, firmware, admin list, device inventory, license state. Use for "what's on their Meraki", wireless/VLAN config questions during triage, or license-expiry checks.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Meraki Read

Reads Meraki dashboard state — networks, devices, wireless, VLANs, admins, licensing — from the Liongard Cisco Meraki inspector's dataprint, so a tech gets environment facts during triage without logging into the dashboard.

## When to use

- "What SSIDs are broadcast at <client>?" / "Which VLAN is the guest wifi on?"
- Wifi or connectivity ticket needs the wireless/VLAN picture before anyone touches the dashboard.
- "Who has admin on their Meraki org?" — access review or offboarding check.
- "When do their Meraki licenses expire?" / "What firmware are the APs on?"
- Post-change incident: "did anything change on the Meraki side?"

## What the inspector captures

Organization and network inventory, device list (MX/MS/MR with models, serials, firmware), wireless SSID configuration, VLAN/subnet layout, organization admins and their privilege levels, and license state/expiration. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Meraki" (a client may have one org-level launchpoint covering many networks), verify last-run success, note dataprint age.
2. Pick the angle from the question and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - SSIDs: something like `Networks[].SSIDs[?Enabled==`true`].{Name:Name, Auth:AuthMode, VLAN:VlanId}` — enabled SSIDs with auth mode and VLAN tagging.
   - VLANs: `Networks[].VLANs[].{Id:Id, Name:Name, Subnet:Subnet}` — the subnet map.
   - Firmware: device list grouped by model + firmware version — flag mixed versions within a model family.
   - Admins: org admin list with privilege ("full" vs "read-only") — count full admins and list names for review.
   - Licensing: license state and expiration date — flag anything expiring within 90 days.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to the Meraki system: SSID/PSK changes, VLAN edits, admin adds/removes, firmware upgrades are the detections that matter — line them up against the incident window.
4. Sanity-check surprising values (zero SSIDs, no admins) — that pattern usually means a wrong field path or a partial inspection, not reality. Re-probe with a broader expression.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (expiring licenses, single-admin risk, firmware drift, unexplained recent changes). Offer a plain-text `add_ticket_note` for the active ticket.

## How this feeds tickets

- **Triage**: attach the SSID/VLAN facts to a wifi ticket so the assigned tech starts with the topology, not a dashboard hunt.
- **Incident correlation**: a wireless outage minutes after a detected SSID or firmware change is a lead — label it time-adjacency, not proven cause.
- **QBR evidence**: license expiry dates, admin counts, and firmware currency are recurring QBR line items — export the facts with their as-of date.

## Guardrails

- Read-only: this skill never edits Meraki config. Changes go through the partner's change process (see devices-and-infrastructure/firewall-rule-change or switch-vlan-change for the workflow side).
- Never paste PSKs or credentials into notes even if the dataprint exposes them — reference "PSK on file in <doc platform>" instead.
- Always state dataprint age; wireless config changes often, so an old dataprint on an active wifi incident deserves an explicit "may be stale — verify live" caveat.
- If no Meraki launchpoint exists, degrade per the access pattern: docs (`search_itglue`/`search_hudu`) → ticket history → "verify on dashboard".
- Admin-list findings are access-review inputs, not accusations — "unrecognized admin, verify" phrasing.
- Plain-text notes only.
