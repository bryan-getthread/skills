---
name: Device Health Check
description: Diagnose a device or a client's fleet through the RMM: alerts, disk, patches, and compliance, then propose remediation.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, list_ninjaone_alerts, search_itglue, add_ticket_note]
---

# Device Health Check

**When to use:** A user's workstation or server is having problems, or someone asks which devices need attention.

**What you get:** A device or fleet health read with risks flagged and concrete remediation offered.

## Steps

1. Identify the device or user. Ask if not given.
2. Pull current alerts and online status.
3. Check disk space, pending reboots, and patch/update and compliance status.
4. Flag risks: low disk, missing critical patches, offline, or repeated alerts.
5. Propose concrete remediation (clear space, schedule reboot, push updates) and offer to action it.

## Guardrails

- Confirm before any change that affects the user or reboots a device.
- Cross-reference inventory when available so you are looking at the right asset.

## Consolidates

Device health check, device health and patch-compliance check, server infrastructure diagnostics, device snapshot, and offline-alert troubleshooting skills.
