---
name: Server Decommission Runbook
description: Safely retire a server — map dependencies, migrate or confirm data, clean up DNS/records/monitoring/backup, wipe, and update documentation — with an approval gate before anything destructive. Use when someone asks to decommission, retire, or "spin down" a server.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_activities, liongard_launchpoint, liongard_metric, liongard_timeline, search_itglue, search_knowledge_base, create_ticket, add_ticket_note, send_approval]
---

# Server Decommission Runbook

Retire a server without breaking the services that quietly depend on it. This is a dependency-first, approval-gated runbook: prove nothing still relies on the box before it is wiped, then clean up every record it touched.

## When to use

- "Decommission <server> — it's being replaced."
- Retiring a physical host, VM, or legacy application server.
- Post-migration cleanup where the old server should be spun down.
- Confirming a server is genuinely safe to power off.

## Steps

1. Identify the server and its role: `search_ninjaone_devices` then `get_ninjaone_device` for OS, role hints, last contact, and installed agents. Pull the asset and its documented purpose from `search_itglue` and any decommission checklist from `search_knowledge_base`.
2. Map dependencies before proposing anything destructive. Check for: roles the box holds (DC/DNS/DHCP/CA/file/print/SQL/app), services other systems consume, scheduled tasks, shares and mapped drives, DNS records pointing at it, backup jobs, and monitoring. Where the partner runs Liongard, use `liongard_launchpoint` to find the relevant inspectors (AD, DNS, DHCP, hypervisor, backup) and `liongard_metric`/`liongard_timeline` to read current config and confirm what references this host. Verify each inspector last ran successfully and state the dataprint age; where no inspector exists, fall back to documentation and RMM and say the dependency check is partial.
3. Check recent activity (`get_ninjaone_device_activities`) for live sessions or users still hitting the box — active use is a stop condition until confirmed.
4. Produce a pre-decommission plan listing, per dependency: what must be migrated, repointed, or confirmed dead first (data migration, DNS/record changes, backup-job removal, monitoring removal, license reclamation). Nothing is marked done that you cannot verify.
5. Gate the destructive phase behind explicit sign-off. Route the plan through `send_approval` so a human authorizes the wipe/power-off; do not represent the shutdown or wipe as something this skill performs.
6. On approval, hand off execution: the actual power-off, disk wipe, and hypervisor/RMM removal are performed by the tech in-console (there is no RMM script-exec or delete path through this skill). Track the record changes (DNS, backup, monitoring, licensing) as they are completed.
7. Close out documentation: mark the asset retired with date and reason, remove it from active inventory, and cross-reference the asset-lifecycle-tracking skill. Post the runbook status to the ticket as a plain-text note (`add_ticket_note`), or `create_ticket` if none exists.

## Guardrails

- Dependency mapping is mandatory before any destructive step — never propose a wipe/power-off until dependencies are accounted for and data is confirmed migrated.
- The wipe, power-off, and deletion are human actions behind `send_approval`; this skill plans, verifies, and documents only. Do not claim to have deleted or wiped anything.
- Trust Liongard/RMM data only after confirming the inspector/agent last ran; always state dataprint/last-contact age. Degrade to docs when a source is absent and say the check is partial.
- Never remove backup jobs or retention until the retention policy for the retired system is confirmed — the data may be needed after the box is gone.
- Notes posted to tickets are plain text — no markdown, no emojis.
