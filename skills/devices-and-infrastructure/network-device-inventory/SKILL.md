---
name: Network Device Inventory
description: Build or refresh the living inventory of a client's network devices — switches, access points, firewalls, routers — per site, by combining documentation with what monitoring actually sees. Use for "what network gear does <client> have", site audits, or when docs and reality have drifted.
category: Devices & Infrastructure
tools: [search_itglue, search_hudu, liongard_environment, liongard_device, liongard_launchpoint, liongard_metric, liongard_timeline, search_ninjaone_devices, get_ninjaone_device, add_ticket_note, create_ticket]
---

# Network Device Inventory

Assemble the per-site picture of a client's network infrastructure — every switch, AP, firewall, and router — by merging documented inventory with monitoring's live view, and flag where the two disagree.

## When to use

- "What network devices does <client> have at <site>?"
- Preparing a network project (refresh, VLAN work, new site) and needing ground truth.
- After an audit finding that documentation is stale.
- A network ticket needs the topology context ("what's between this user and the firewall?").

## Steps

1. Pull the documented inventory: `search_itglue` / `search_hudu` for the client's network device configurations, per site. Capture device name, role (switch/AP/firewall/router), make/model, site/location, and management IP where documented — but never credentials, even if the doc contains them.
2. Pull the observed inventory: Liongard reaches most network platforms (Meraki, UniFi, firewall and switch inspectors) — use `liongard_launchpoint` filtered by system type to find which inspectors exist and confirm each last ran successfully, then `liongard_device` / `liongard_metric` for the device data and `liongard_timeline` for firmware and change history, stating dataprint age per source; `search_ninjaone_devices` for anything with an SNMP/agent presence in the RMM. Note last-seen times; skip inspectors whose last run failed and list them as blind spots.
3. Merge per site into one table: device, role, make/model, documented?, monitored?, last seen, firmware where available. Match on management IP first, then name; flag low-confidence matches.
4. Flag the drift, in three buckets: documented-but-not-monitored (monitoring blind spot or retired gear), monitored-but-not-documented (documentation gap — the more dangerous one), and conflicts (docs say one model/IP, monitoring says another).
5. Enrich with lifecycle signals where data allows: firmware age, devices past end-of-support (verify EOL dates against the vendor's published lifecycle pages rather than memory), single points of failure evident from the inventory (one switch for a whole site, no redundant firewall).
6. Output the per-site inventory table plus a drift/risk list with recommended actions (document X, add monitoring for Y, verify Z on site). Offer to open a documentation-update ticket (`create_ticket`) — updating the documentation platform itself is a handoff, not something this skill writes — and post a plain-text summary note (`add_ticket_note`).

## Guardrails

- Never copy credentials, SNMP community strings, or pre-shared keys into your output, even when documentation exposes them. Reference the doc by name/location instead.
- The merged inventory is only as fresh as its sources — always state the last-seen / last-updated times and never present the table as verified-complete without a site walk.
- Do not invent make/model/firmware values to fill table gaps; leave cells empty and list them as unknowns.
- If neither documentation nor Liongard/RMM data is available for the tenant, say so — an inventory cannot be fabricated from ticket mentions alone (though ticket history may suggest devices to go verify).
- Notes posted to tickets are plain text — no markdown, no emojis.

## See also

- `rmm-cross-tool-reconciliation` — the generic two-inventory diff this skill applies to network gear.
- `wifi-infrastructure-audit` — the AP-specific deep dive.
- `network-outage-triage` — when the question is "what's down right now" rather than "what exists".
