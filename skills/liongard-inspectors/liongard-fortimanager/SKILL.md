---
name: Liongard FortiManager Read
description: Interrogate a client's FortiManager through the Liongard FortiManager inspector — managed FortiGate devices, ADOMs, policy packages, firmware, admin access. Use for "what does FortiManager manage for them?", fleet firmware-currency checks, or centrally-managed policy questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard FortiManager Read

Reads FortiManager state — managed FortiGate devices, ADOMs, policy packages, firmware, and admin access — from the Liongard FortiManager inspector's dataprint, so a tech gets the centrally-managed Fortinet picture during triage without logging into FortiManager. For a single firewall's live rule base, prefer the per-device FortiGate inspector (liongard-fortigate).

## When to use

- "What FortiGates does FortiManager manage at <client>?" — fleet inventory.
- "What firmware is the Fortinet fleet on?" — currency/vulnerability check across devices.
- "What ADOMs / policy packages are configured?" — centrally-managed policy context.
- "Which managed devices are out of sync / offline?"
- Post-change: "did a policy package or managed device change on FortiManager?"

## What the inspector captures

Managed FortiGate device inventory (model, firmware, sync/connection status), ADOM structure, policy packages, and FortiManager admin access. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "FortiManager", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Devices: managed-device list with model and firmware — flag firmware drift / outdated builds.
   - Sync: devices out-of-sync or disconnected.
   - ADOMs/packages: ADOM list and policy-package assignment.
   - Admins: FortiManager admin accounts.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to FortiManager: device adds/removes, policy-package changes, firmware upgrades, admin changes — line them up against the incident window.
4. Sanity-check surprising values (zero devices) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (firmware drift, out-of-sync devices, unexpected admins). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach the managed-fleet picture to a Fortinet ticket; drill into a single device via liongard-fortigate.
- **Currency review**: fleet-wide firmware drift is a recurring security finding — surface with the as-of date, recommend the tech confirm live.
- **Incident correlation**: an issue after a detected policy-package push is a lead — label it time-adjacency, not proven cause.

## Guardrails

- Read-only: this skill never edits FortiManager config, policy packages, or pushes to devices. Changes go through the partner's change process.
- Never paste credentials or VPN secrets into notes even if the dataprint exposes them — reference "credentials on file in <doc platform>".
- Always state dataprint age; managed-fleet state changes — an old dataprint on an active incident deserves a "verify live" caveat.
- If no FortiManager launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in FortiManager". Also check for standalone FortiGate inspectors (liongard-fortigate).
- Plain-text notes only.
