---
name: NinjaOne Alert Types
description: A NinjaOne-native condition/threshold alert landed — classify the alert type (offline, resource threshold, service, patch, hardware/health, security), read live device state with the real NinjaOne tools, and route each class to its runbook with a deep-link handoff.
category: Vendor Runbooks
tools: [list_ninjaone_alerts, search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_activities, get_ninjaone_device_link, add_ticket_note]
---

# NinjaOne Alert Types

Unlike most vendor runbooks, NinjaOne has real Super Magic read tools — so this skill reads *live* device state instead of inferring it. NinjaOne's native alerts are condition/threshold based (a monitored condition crossed a threshold or a state changed), and the class of alert decides the response. This skill is the front door: classify the NinjaOne-native alert, confirm the current condition with the live tools, and route each class to the runbook that owns it — the devices-and-infrastructure alert skills (device-offline-runbook, disk-space-remediation, alert-reset-with-note, hypervisor-alerts, fleet-health-sweep) for the operational classes, and the security runbooks for the security classes. Remediation on the endpoint is a technician action reached via a deep link — NinjaOne here is read + reset + deep-link, never script execution. Verify condition/alert names against NinjaOne's current documentation.

## When to use

- A NinjaOne condition/threshold alert arrives (device offline, disk/CPU/memory threshold, stopped service, patch condition, hardware/SMART/RAID health, security/AV condition)
- A tech asks what a NinjaOne alert means or wants current device state read
- Recovered NinjaOne alerts need verifying and clearing (route to alert-reset-with-note)

## Steps

1. Enumerate the exact alert(s) with `list_ninjaone_alerts` — device, alert type, message, timestamp. If several match a fuzzy description, list them and confirm which; never act on a fuzzy match.
2. Classify the alert type into a response lane:
   - Device-offline → device-offline-runbook.
   - Disk-space → disk-space-remediation.
   - CPU/memory/performance threshold → performance triage.
   - Stopped service → service-health check.
   - Patch condition → the patching runbook.
   - Hardware / SMART / RAID / health → hardware-failure path (never auto-cleared).
   - Security / AV / EDR condition → security-alert-response (leaves the operational lane entirely).
3. Read live state to confirm the condition is real *now* — the alert text is a claim; `get_ninjaone_device` gives current online status, disk, resource, and last-contact, and `get_ninjaone_device_activities` gives the recent timeline around the alert. Distinguish a still-active condition from one that already recovered. (`search_ninjaone_devices` resolves the device if the alert lacks a clean handle.)
4. Recurrence check: if the same alert has fired and cleared repeatedly (3+ in 30 days), flag it as a chronic condition needing a root-cause ticket rather than a routine acknowledgement — don't feed alert-blindness.
5. Route and hand off: pass the finding into the class's runbook. Any on-endpoint remediation (script, service control beyond a supported action, software, reboot as a fix) is a technician action reached via `get_ninjaone_device_link` — the agent deep-links the tech in; it does not run scripts, deploy software, or push policy. For a genuinely recovered, allowlisted operational alert, closing the loop (verify-note-reset) belongs to alert-reset-with-note — use that skill, don't reset security/hardware/backup alerts here.
6. Post a plain-text note: alert type and class, live-state confirmation (with the reading), recurrence status, the runbook it routed to, and any deep-link handoff. Do not fabricate device details — only what the tools returned.

## Guardrails

- Classify first, then confirm with live state — the alert message is a claim; `get_ninjaone_device` decides whether the condition is still real.
- NinjaOne is read + reset + deep-link only: the agent cannot run scripts, deploy software, or push policy — remediation is a technician step reached via `get_ninjaone_device_link`.
- Security, hardware-health, RAID/SMART, backup, and domain-controller/hypervisor alerts leave the operational lane — route to the security/hardware runbooks and never auto-clear them here.
- Alert reset is a separate, gated action — hand recovered operational alerts to alert-reset-with-note (verify-then-note-then-reset); never reset to make a queue look clean.
- Recurrence (3+ in 30 days) → root-cause ticket, not another silent acknowledgement.
- If NinjaOne is not enabled for the tenant, this skill cannot run — say so.
- Cross-reference, don't duplicate: the devices-and-infrastructure alert skills own the per-class investigation; this skill classifies, confirms live, and routes.
