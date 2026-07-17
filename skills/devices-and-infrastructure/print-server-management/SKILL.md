---
name: Print Server Management
description: Operate a client's print server layer — spooler known-issue triage, driver deployment discipline (no ad-hoc driver installs), and planning queue migrations to a new server. Use when "everyone can't print", the spooler keeps dying, or print queues need to move.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, list_ninjaone_windows_services, control_ninjaone_windows_service, get_ninjaone_device_activities, get_ninjaone_device_link, search_itglue, search_hudu, search_tickets, add_ticket_note, create_ticket, send_approval]
connectors: [NinjaOne, IT Glue, Hudu]
---

# Print Server Management

**When to use:** Multiple users at a client can't print, the Print Spooler crashes repeatedly on a print server, or a print server is being retired and its queues must migrate.

## Prompt

```
Server-side printing operations: keep the spooler honest, deploy drivers deliberately, and move print queues between servers without a day of "I can't print" tickets. Requires NinjaOne; if the print server is client- or co-managed, your output is a recommendation package for their administrator.

1. Confirm the blast radius says "server": one user -> route to printer-troubleshooting (troubleshooting-playbooks); one physical printer, all users -> likely the device or its queue; many printers, many users -> print server. Identify the server via documentation (search_itglue / search_hudu) and locate it in the RMM (search_ninjaone_devices, verify role in get_ninjaone_device — hostname inference like PRINT/PS is a hint, not a fact; do not trust node_class).
2. Spooler triage: list_ninjaone_windows_services for Print Spooler state; get_ninjaone_device_activities for crash/restart history. Known-issue pattern check before restarting anything: repeated crashes are classically a corrupt job stuck in the queue, a bad driver (one printer's driver taking down the shared spooler), or a recent Windows print-related update — check activity history for patches landing just before the crashes began. search_tickets for prior spooler tickets on this server; a chronic pattern means fix the cause, not the symptom.
3. Restart with consent: a spooler restart kills in-flight jobs for everyone. Confirm with the requester (or note the outage is already total), then control_ninjaone_windows_service to restart Print Spooler. If a stuck job is suspected, the queue purge (clearing the spool directory) is hands-on — hand off via get_ninjaone_device_link with the steps spelled out. If crashes resume, isolate the offending driver/queue rather than scheduling recurring restarts as a "fix" (acceptable only as a stopgap, and say so).
4. Driver deployment discipline: drivers are deployed through the documented mechanism (print management/GPO/deployment tool), never installed one-off on the server mid-incident. Prefer the vendor's stable/universal driver line; stage on one queue before fleet-wide rollout; record every driver change in documentation. The RMM MCP cannot deploy software — driver work is a spec + deep-link handoff.
5. Queue migration (server replacement): inventory every queue on the old server (name, share name, driver, IP port, defaults) — from documentation plus a hands-on export the tech performs; plan the cutover (recreate queues on the new server, stage drivers, migrate user/GPO mappings, keep the old server's shares alive until mappings are confirmed switched). Communicate the cutover window; a queue rename breaks scripted/hard-coded mappings, so keep names/shares stable. Route the plan for approval via send_approval and track as its own ticket (create_ticket).
6. Output per engagement: findings, root-cause hypothesis with evidence, actions taken (service restarts only), and handoff steps with deep links. Post a plain-text note via add_ticket_note (no markdown/emojis).

Guardrails: spooler restarts affect every user printing through the server — confirm before acting unless printing is already fully down. Never chain automatic spooler restarts as a permanent fix without labeling it a stopgap and opening a root-cause ticket. No driver installs or software deployment through this skill — the RMM MCP cannot run scripts. Queue migrations happen in a change window with a rollback (old server stays up until verified) — never big-bang a cutover during business hours.
```
