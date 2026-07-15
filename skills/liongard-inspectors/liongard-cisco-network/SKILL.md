---
name: Liongard Cisco Network Read
description: Interrogate a client's Cisco switches/routers (IOS/IOS-XE) through Liongard — IOS versions across the fleet, running-config change detections, port/interface inventory, VLAN layout. Use for "what changed on the switch", IOS-currency sweeps, or port/VLAN facts during connectivity triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Cisco Network Read

Reads Cisco switch/router state — IOS version, running configuration, interfaces, VLANs — from the Liongard inspector per device, with config-change detections between runs standing in for "who touched the switch". (Meraki gear is a different inspector — use liongard-inspectors/liongard-meraki.)

## When to use

- "Did the switch config change before this VLAN broke?" — config-diff correlation.
- "What IOS versions are across <client>'s switches?" — currency/advisory sweep before an upgrade project.
- Connectivity ticket: "what VLAN is that port on / is the trunk carrying it?"
- Port-capacity question: free ports before a desk move or expansion.
- Documented-vs-actual VLAN check (pair with liongard-inspectors/liongard-network-documentation-sync).

## What the inspector captures

Per-device: hostname/model/serial, IOS/IOS-XE version, interface inventory with descriptions and VLAN assignments, VLAN database, and configuration snapshots that drive change detections between inspector runs. Depth varies by platform and inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve environment, `liongard_launchpoint` with systemType "Cisco" (naming varies — try "Cisco IOS"/"Cisco Switch"/"Cisco Router" variants), verify last-run success per device, note dataprint age. Expect one launchpoint per device; a fleet question means iterating them all — say how many you covered.
2. Query the angle via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - IOS sweep: version string per device, grouped — the drift table (same model, different IOS = upgrade-planning input).
   - VLANs: VLAN id/name list per switch; compare across switches for VLANs defined on one side of a trunk only.
   - Ports: interface list with description, access VLAN, and status fields if present — free-port counts come from unconfigured/administratively-down interfaces, clearly labeled as an approximation of "free".
3. **Config changes**: `liongard_detection` / `liongard_timeline` per device — running-config change detections are the core value. Order against the incident window; an unexplained config change on a switch feeding the affected area is the lead.
4. Fleet-wide questions: aggregate per-device answers into one table; flag devices whose inspector hasn't run recently as "stale — excluded/qualified" rather than silently mixing ages.
5. Output: table (device / fact / as-of), change list where relevant, flags (IOS drift, EOL-looking versions labeled "verify against Cisco EOL notices", VLAN mismatches, stale inspectors). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: port/VLAN facts attached to connectivity tickets before anyone consoles in.
- **Incident correlation**: config-change detections vs outage window — time-adjacency, bounded by inspector run cadence, not a precise change log.
- **Projects/QBR**: the IOS drift table and EOL flags feed upgrade projects and QBR posture slides; free-port approximations feed expansion planning.

## Guardrails

- Read-only; config changes go through change control (devices-and-infrastructure/switch-vlan-change for the workflow side).
- Interface up/down state in a dataprint is a snapshot, not monitoring — never present it as current link state during an active incident; the RMM or on-device check owns "is it up right now".
- "Free ports" from config is an approximation (a port can be unconfigured and physically in use) — label it.
- Report IOS versions verbatim; EOL/vulnerability mapping needs verification against Cisco notices, not memory.
- Never reproduce enable secrets, SNMP strings, or key material that may appear in config-derived data.
- Degrade per the access pattern when devices lack inspectors; partial fleet coverage must be stated ("6 of 9 documented switches have inspectors").
- Plain-text notes only.
