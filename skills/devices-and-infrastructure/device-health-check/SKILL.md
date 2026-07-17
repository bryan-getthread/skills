---
name: Device Health Check
description: Diagnose a single device through the RMM — alerts, activities, services, disk, reboot, and patch posture — and propose remediation with a deep-link handoff. Use when a user's workstation or server is misbehaving or someone asks "what's wrong with this machine".
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_activities, list_ninjaone_alerts, list_ninjaone_windows_services, get_ninjaone_device_link, search_itglue, add_ticket_note]
connectors: [NinjaOne, IT Glue]
---

# Device Health Check

**When to use:** "<user>'s computer is slow/freezing — what does the RMM say?", a ticket names a device by hostname, or you need a baseline before touching a machine.

## Prompt

```
One-pass health read of a single device, ending in a concrete remediation proposal the tech can act on through a deep link. Read-only skill. Requires NinjaOne.

1. Resolve the device without stopping to ask mid-lookup: identify the client organization from the ticket/prompt, then search_ninjaone_devices within it. If multiple candidates match, rank by organization match first, then most recent last-contact, and state which one you picked and why. Only ask if candidates are genuinely indistinguishable.
2. Pull in parallel: get_ninjaone_device (status, OS, last contact, disk, pending reboot), list_ninjaone_alerts scoped to the device, get_ninjaone_device_activities (last 24-72h), and list_ninjaone_windows_services for stopped services set to Automatic.
3. Infer the device's role from its hostname prefix where the convention is evident (DC/AD = domain controller, SQL/DB = database, FS = file server, RDS/TS = terminal server, HV/ESX = hypervisor). Say it is an inference, not a fact.
4. Check recent activities for another tech already working the device (remote sessions, manual actions). If someone is active, flag it before proposing anything.
5. Co-hosted-VM contention check: if the device shares a public IP with other devices in the same organization, list those siblings and check whether any carries resource alerts.
6. Assess posture: disk free space (flag under ~15% or under 10 GB on system volume), pending reboot age, patch status and failed-patch activities, repeated/flapping alerts.
7. Cross-reference documentation (search_itglue) when available to confirm the asset and pick up known quirks.
8. Output a health summary: status line, risks ranked by severity, inferred role, and a remediation proposal per risk. For any hands-on remediation, provide get_ninjaone_device_link so the tech jumps straight to the device. Offer to post the summary as a plain-text note via add_ticket_note (no markdown/emojis).

Guardrails: do not trust the node_class filter — verify the returned device's class in its details before reasoning about it as a server or workstation. If any listing may have capped, report counts as "at least N", never exact. This skill is read-only; actions (reboot, service restart) belong to the dedicated action skills and require requester confirmation before affecting a logged-in user. Role inference from hostname is a hint, never confirmed without documentation backing. If NinjaOne is not enabled, say so and fall back to ticket history and documentation only; do not fabricate device data.
```
