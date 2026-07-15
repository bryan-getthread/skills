---
name: Device Health Check
description: Diagnose a single device through the RMM — alerts, activities, services, disk, reboot, and patch posture — and propose remediation with a deep-link handoff. Use when a user's workstation or server is misbehaving or someone asks "what's wrong with this machine".
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_activities, list_ninjaone_alerts, list_ninjaone_windows_services, get_ninjaone_device_link, search_itglue, add_ticket_note]
---

# Device Health Check

One-pass health read of a single device: current alerts, recent activity, service state, and disk/reboot/patch posture, ending in a concrete remediation proposal the tech can act on through a deep link.

## When to use

- "<user>'s computer is slow / freezing / acting up — what does the RMM say?"
- A ticket references a device by hostname and you need its current state.
- Before touching any device (reboot, service restart), to establish a baseline.
- "Is anything wrong with <device>?"

## Steps

1. Resolve the device without stopping to ask mid-lookup: identify the client organization from the ticket or prompt, then `search_ninjaone_devices` within it. If multiple candidates match, rank by organization match first, then most recent last-contact time, and state which one you picked and why. Only ask the user if candidates are genuinely indistinguishable.
2. Pull in parallel: `get_ninjaone_device` (status, OS, last contact, disk, pending reboot), `list_ninjaone_alerts` scoped to the device, `get_ninjaone_device_activities` (last 24–72h), and `list_ninjaone_windows_services` for stopped services set to Automatic.
3. Infer the device's role from its hostname prefix where the naming convention is evident (e.g. DC/AD = domain controller, SQL/DB = database, FS = file server, RDS/TS = terminal server, HV/ESX = hypervisor). Say it is an inference, not a fact.
4. Check recent activities for another tech already working the device (remote sessions, recent manual actions). If someone is active on it, flag that before proposing anything.
5. Co-hosted-VM contention check: if the device shares a public IP with other devices in the same organization, list those siblings and check whether any of them carries resource alerts — contention on a shared host can be the real cause.
6. Assess posture: disk free space (flag under ~15% or under 10 GB on the system volume), pending reboot age, patch status and failed-patch activities, repeated or flapping alerts.
7. Cross-reference documentation (`search_itglue`) when available to confirm you are looking at the right asset and to pick up known quirks.
8. Output a health summary: status line, risks ranked by severity, inferred role, and a remediation proposal per risk. For any hands-on remediation, provide `get_ninjaone_device_link` so the tech can jump straight to the device. Offer to post the summary to the ticket as a plain-text note via `add_ticket_note`.

## Guardrails

- Do not trust the node_class filter on search results — verify the returned device's class in its details before reasoning about it as a server or workstation.
- If any listing may have hit a result cap, report counts as "at least N", never as exact.
- Confirm with the requester before any change that affects a logged-in user (reboot, service restart). This skill itself is read-only; actions belong to the reboot/service/maintenance skills.
- Role inference from hostname is a hint — never present it as confirmed without documentation backing it.
- If NinjaOne is not enabled for this tenant, say so and fall back to ticket history and documentation only; do not fabricate device data.
- Notes posted to tickets are plain text — no markdown, no emojis.

## Consolidates

Device health check, device health and patch-compliance check, server infrastructure diagnostics, device snapshot, and offline-alert troubleshooting skills.
