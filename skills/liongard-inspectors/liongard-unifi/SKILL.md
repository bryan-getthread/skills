---
name: Liongard UniFi Read
description: Interrogate a client's Ubiquiti UniFi controller through the Liongard inspector — device inventory, adoption state, firmware drift, WLAN/network config. Use for "what UniFi gear do they have", wifi triage context, or firmware-currency checks.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard UniFi Read

Reads UniFi controller state — sites, adopted devices, firmware versions, WLANs, network/VLAN config — from the Liongard Ubiquiti UniFi inspector, giving techs the environment picture without controller access.

## When to use

- "What UniFi devices does <client> have?" — inventory for triage or a refresh quote.
- Wifi ticket: which APs serve the site, what WLANs exist, what auth they use.
- "Are their UniFi devices on current firmware?" — drift check before blaming firmware bugs.
- "Is anything disconnected/not adopted on their controller?"
- Change correlation after a controller upgrade or WLAN edit.

## What the inspector captures

Controller/site inventory, adopted devices (APs, switches, gateways — model, name, IP, firmware, adoption/connection state), WLAN configuration (SSID, security mode, associated network), and network/VLAN definitions. Self-hosted vs cloud controllers may expose different depth; coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve environment, `liongard_launchpoint` with systemType "UniFi" (also try "Ubiquiti" if empty — naming varies), verify last-run success, note dataprint age. Multi-site controllers: identify which site maps to this client before answering.
2. Query the angle via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Inventory: devices grouped by type/model with name, IP, firmware — the "what's there" table.
   - Firmware drift: group devices by model, compare firmware versions within each model — same-model boxes on different firmware is the drift finding.
   - Connection state: devices where state != connected/adopted — offline or orphaned gear.
   - WLANs: enabled WLANs with SSID, security mode (flag open or WEP/WPA1 networks), and network binding.
3. For "what changed": `liongard_detection` / `liongard_timeline` — device adds/removals, firmware upgrades, WLAN edits around the incident window.
4. Output: inventory/config table, source + dataprint age, and flags (offline devices, firmware drift, weak WLAN security, single points of failure like one AP per site). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: "user can't get on wifi" tickets start with the AP + WLAN facts attached instead of a controller login.
- **Incident correlation**: an outage following a detected controller/firmware change is a lead (time-adjacency, not proven cause).
- **QBR / refresh evidence**: device counts, models, and firmware currency feed hardware-refresh forecasting (cross-ref devices-and-infrastructure/hardware-refresh-forecast).

## Guardrails

- Read-only; no controller writes. Adoption, upgrades, and WLAN changes are tech-owned.
- UniFi dataprints reflect the controller's view — a device the controller lost may still be powered and forwarding. Phrase as "controller reports disconnected", not "device is down".
- Never expose wifi PSKs in notes; point to the documentation platform.
- State dataprint age always; degrade to docs/ticket-history per the access pattern when the inspector is absent.
- Plain-text notes only.
