---
name: Service Restart Runbook
description: Restart a crashed or stopped Windows service through the RMM — allowlisted safe services only, with state verified before and after and a note posted. Use for "the print spooler died on <device>" or a stopped-service alert, attended or embedded in a Flow.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, list_ninjaone_windows_services, control_ninjaone_windows_service, add_ticket_note]
---

# Service Restart Runbook

Restarts a stopped Windows service via `control_ninjaone_windows_service` with the full discipline: verify it is actually stopped, check it is safe to touch, restart, verify it stayed up, write it down.

## When to use

- A stopped-service alert fires (spooler, agent services, app services).
- "Restart the <service> on <device>" from a tech or a ticket.
- A Flow wants known-safe crashed services bounced automatically (see Unattended variant).

## Steps

1. Resolve organization then device via `search_ninjaone_devices`; rank by org match then last-contact; verify device class in `get_ninjaone_device` details — do not trust the node_class filter. Note whether the device is a server and whether users are active on it.
2. Verify current state with `list_ninjaone_windows_services`: the named service must actually be stopped or hung. If it is running, report that and stop — restarting a healthy service is an outage.
3. Check the service against the safety tiers:
   - Safe allowlist (restart with normal confirmation; unattended-eligible): Print Spooler, Windows Update, BITS, Windows Time, DHCP Client, DNS Client, workstation-level agent/monitoring services, and application services the tenant has explicitly allowlisted.
   - Domain-critical / never unattended, strong confirmation attended: Active Directory Domain Services, DNS Server, DHCP Server, database engines (SQL Server and kin), Exchange services, hypervisor/VM services, cluster services, backup engines, certificate services.
   - Unknown services: treat as critical until identified.
4. Check `get_ninjaone_device` activities/state for another tech already working the device — a stopped service may be intentionally stopped mid-maintenance. If maintenance mode is active, do nothing.
5. Attended path: confirm with the requester if the device is a server, users are active, or the service is outside the safe allowlist. Then `control_ninjaone_windows_service` to start/restart.
6. Verify after: re-pull `list_ninjaone_windows_services` and confirm the service is running. If it stops again immediately, do NOT loop restarts — a crash-looping service is a root-cause problem; escalate with the evidence.
7. Post a plain-text note via `add_ticket_note`: device, service, state found, action taken, state after, and any recurrence observed. Report the same in your reply.

## Guardrails

- Never restart domain-critical services unattended, and attended only with explicit human confirmation that names the blast radius.
- One restart attempt per session — a service that will not stay up gets escalated, not hammered.
- A service stopped on purpose (maintenance, tech activity, disabled startup type) is not a fault; check startup type and context before acting.
- Verify state before AND after; the note is mandatory — no silent service control.
- If NinjaOne is not enabled, this skill cannot run; say so.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the ticket note — plain text, no narration, no markdown.
- Deterministic gates, all required: service is on the safe allowlist; current state verified stopped; device not in maintenance mode; no restart of this same service by automation in the past 24h (crash-loop guard); startup type is Automatic. Any gate fails → do nothing and output one plain-text line stating which gate failed.
- Never touch domain-critical or unknown services unattended under any phrasing of the trigger.
- After restart, verify running state and include found/after states in the note. If verification shows it stopped again, output the escalation line — do not retry.
