---
name: Level.io RMM Alerts
description: A Level.io RMM alert or ticket landed — triage the monitored condition (offline, resource, patch, script-monitor) from the alert and ticket history, and hand the technician into the Level console for any action. Read/context only — no remote script execution.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_itglue, add_ticket_note, update_ticket]
---

# Level.io RMM Alerts

Vendor specialization of device-alert-triage for Level.io, a lightweight RMM. Level.io has no native Super Magic integration today, so this skill is a read/context-and-handoff workflow: the agent triages the condition from the alert text, the ticket, documentation, and any Liongard inspector that covers the client's estate, then directs the technician into the Level console. The agent does not — and cannot — run Level scripts, deploy software, or push policy. The device-alert generic runbooks own the triage canon; this skill adds Level.io's alert vocabulary and the honest capability boundary. Verify monitor/alert names against Level.io's current documentation.

## When to use

- A Level.io alert arrives as a ticket: device offline, CPU/memory/disk threshold, patch status, or a custom script-monitor condition
- A tech asks how to read a Level.io alert or what to check next
- Level.io device context is needed for another investigation

## Steps

1. Parse the alert anatomy: device/host, the monitored condition and its threshold, current vs alerting value, first-seen time, and any recovery signal. Copy Level.io's exact monitor wording. Route to the client per the desk's routing rules; low confidence → flag for a human.
2. Establish state from what's available — the agent has no Level.io read tool, so triage from: the alert payload, ticket history (search_tickets, same device/condition, recent window) for recurrence, client documentation (search_itglue), and, if the client runs a Liongard inspector covering the asset, Liongard reads for corroborating config/state (verify the inspector last ran successfully and state its dataprint age; degrade to docs/ticket-history if absent).
3. Branch by condition onto the matching generic runbook: offline → device-offline-runbook; disk → disk-space-remediation; CPU/memory → performance triage; patch → the patching runbook; custom script-monitor → read the monitor's intent from documentation and treat unknown monitors as "meaning unconfirmed" rather than guessing.
4. Recurrence and noise: if the same alert has fired repeatedly, flag it as a chronic condition needing a root-cause ticket rather than repeated acknowledgement — do not train the desk to ignore it.
5. Hand off for action: any remediation (script run, service restart, reboot, software deploy, policy change) happens in the Level.io console and is a technician action. Write the handoff note with exactly what to do and what to verify; the agent directs and records, it does not execute.
6. Document condition, state assessment (and its source/age), recurrence, and the handoff. Client-facing wording per defensive-writing-standard.

## Guardrails

- The agent cannot run Level.io scripts, deploy software, reboot devices, or push policy — every action is a technician step in the Level console that the agent directs and records. Never imply the agent performed a remediation.
- No native Level.io read integration: be explicit that state is inferred from alert/ticket/docs/Liongard, and state the source and freshness — never present inferred state as a live console read.
- Do not guess what a custom script-monitor means; verify against documentation and mark unconfirmed meanings as unconfirmed.
- Security/backup/hardware-health alert types are never quietly acknowledged or reset — escalate per the security and backup runbooks.
- Recurrence gets a root-cause ticket, not repeated silent acknowledgement.
- Degradation: without documentation or a Liongard inspector, device context may be limited to the alert text — say so.
