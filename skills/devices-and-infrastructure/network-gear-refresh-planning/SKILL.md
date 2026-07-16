---
name: Network Gear Refresh Planning
description: Plan a switch / access-point / firewall refresh — inventory the gear, check EOL and warranty, assess capacity headroom, and confirm configs are backed up before any swap. Use when someone asks to plan a network refresh, budget for new switches/APs/firewalls, or "what network gear needs replacing".
category: Devices & Infrastructure
tools: [liongard_launchpoint, liongard_metric, liongard_timeline, search_ninjaone_devices, search_itglue, web_search, create_ticket, add_ticket_note]
---

# Network Gear Refresh Planning

Turn a site's switches, APs, and firewalls into a defensible refresh plan: what is end-of-life or out of warranty, what is running short on capacity, and what must be config-backed-up before anyone touches it.

## When to use

- "Which network gear at <client> needs replacing this year?"
- Budgeting a switch/AP/firewall refresh for a site.
- A vendor EOL notice landed and you need the exposure at a client.
- Pre-refresh planning before ordering replacement hardware.

## Steps

1. Inventory the network gear for the site. Where Liongard runs, use `liongard_launchpoint` filtered by systemType (e.g. "Meraki", "Palo Alto", "Fortinet", "Ubiquiti", "Cisco") to enumerate each device's launchpoint, model, firmware, and last-run state; cross-reference documented inventory (`search_itglue`) and any RMM-visible network devices (`search_ninjaone_devices`). Confirm each inspector last ran successfully and state the dataprint age. Cross-reference the existing network-device-inventory skill for the inventory pass.
2. For each device, establish lifecycle facts: model, install/purchase date, firmware version, warranty/support-contract status, and vendor EOL/EOS dates. Confirm EOL/EOS against the vendor's current published lifecycle pages with `web_search` — do not rely on memory for dates. Cross-reference the warranty-eol-report and hardware-refresh-forecast skills for the lifecycle data those already assemble.
3. Assess capacity headroom where the data is readable: switch port utilization (used vs. available, PoE budget), AP client density and coverage gaps, firewall throughput/licensing headroom (`liongard_metric` against the relevant inspector dataprint). State where headroom cannot be measured rather than guessing.
4. Rank each device: Replace now (EOL + out of warranty, or capacity-constrained), Replace this cycle (approaching EOL or warranty expiry), or Monitor. Note firmware that is behind but the hardware is still supported as a patch item, not a refresh.
5. Config-backup-first gate: before any refresh proceeds, confirm each device's current configuration is captured — hand this to the firewall-config-backup-audit skill for firewalls, and verify switch/AP configs are exported/backed up. Flag any device with no current config backup as a hard blocker on its refresh line item.
6. Output a refresh plan table: device, model, lifecycle status, warranty, capacity note, ranking, config-backup state, and recommended action with an approximate window. Post to the ticket as a plain-text note (`add_ticket_note`) or `create_ticket` for the refresh project.

## Guardrails

- Verify EOL/EOS/warranty dates against current vendor sources — never state a lifecycle date from memory.
- Trust Liongard/RMM data only after confirming the inspector/agent last ran; report dataprint age. Where a device has no inspector and no documentation, list it as "unverified — inventory only" rather than inferring capacity or firmware.
- Config-backup confirmation is a prerequisite for every refresh line; do not present a device as ready to swap without it.
- This is planning only — it does not change any device config or push firmware. Actual changes route through switch-vlan-change, firewall-rule-change, and the vendor console.
- If a device may have been truncated by a result cap, report counts as "at least N".
- Notes posted to tickets are plain text — no markdown, no emojis.
