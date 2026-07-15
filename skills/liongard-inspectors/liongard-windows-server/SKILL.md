---
name: Liongard Windows Server Read
description: Interrogate a client's Windows Servers through Liongard — installed roles, local admin members, services, patch level, OS version and EOL flags. Use for "what does this server do", local-admin audits, EOL-OS sweeps, or patch-posture checks per server.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Windows Server Read

Reads per-server Windows state — OS version/build, roles and features, local administrator membership, services, installed updates — from the Liongard Windows inspector. The workhorse reads: "what is this server actually doing", "who is a local admin", and "which servers are running an EOL OS".

## When to use

- Triage lands on a server: "what roles/services does <device> run?" before touching it.
- "Who's in the local Administrators group on <device> / across the fleet?" — access audit or offboarding sweep.
- "Any servers still on an end-of-life OS?" — the EOL sweep for risk review or upgrade projects.
- "When was <device> last patched / what build is it on?" — patch-posture spot check.
- "What changed on this server?" — new service, new admin, role change around an incident.

## What the inspector captures

OS version/build and install date, installed roles and features, local users and group membership (Administrators group especially), services with state and start mode, installed hotfixes/updates, and hardware/identity basics. Depth varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve environment, `liongard_launchpoint` with systemType "Windows" (one launchpoint per server), verify last-run success per server, note dataprint age. Fleet sweeps iterate all Windows launchpoints — state coverage ("14 of 16 documented servers have working inspectors").
2. Query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Identity/EOL: OS caption + build per server — flag versions at or past end of support, labeled "verify against Microsoft lifecycle dates" rather than asserting from memory.
   - Roles: installed roles/features — the "what does this box do" answer (AD DS, DNS, DHCP, IIS, Hyper-V, file services…).
   - Local admins: Administrators group members per server — flag unexpected user accounts, per-user counts across the fleet (one account admin on every server is a finding), and non-standard service accounts.
   - Services: services in a stopped state whose start mode is Automatic — the classic silent-breakage list; also new/unrecognized services.
   - Patch level: latest installed update date/build — servers >60 days behind get flagged; deep patch analysis belongs to the RMM (devices-and-infrastructure/patch-compliance-review) — this is the Liongard-side approximation, label it as such.
3. "What changed": `liongard_detection` / `liongard_timeline` per server — new local admin, service adds/state changes, role changes ordered against the incident window. An undocumented local-admin addition is a flag even with no incident.
4. Output: per-server or fleet table for the asked angle, flags with severity (EOL OS, unexpected admins, stopped-auto services, stale patching), source + dataprint age per server. Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: server tickets open with roles/services facts — the tech knows what the box does before RDPing in.
- **Incident correlation**: service or admin changes vs incident window (time-adjacency, bounded by inspector cadence).
- **QBR / risk**: the EOL-OS list and local-admin hygiene stats are recurring QBR and security-review lines; EOL findings feed devices-and-infrastructure/warranty-eol-report and win11-readiness-assessment style deliverables.

## Guardrails

- Read-only: never start/stop services or change group membership — service control via RMM is a separate, gated workflow (devices-and-infrastructure/service-restart-runbook).
- Local-admin findings are "unrecognized — verify" access-review inputs, never accusations; some entries are legitimate service accounts.
- Patch posture from Liongard approximates; the RMM/patch platform is authoritative for compliance claims — say which source answered.
- EOL claims name the OS/build and defer the lifecycle date to verification against Microsoft's published lifecycle.
- State dataprint age per server; a fleet table mixing ages must show the as-of per row or qualify the whole table.
- Degrade per the access pattern (docs → ticket history → RMM) for servers without inspectors.
- Plain-text notes only.
