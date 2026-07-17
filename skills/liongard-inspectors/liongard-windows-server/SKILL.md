---
name: Liongard Windows Server Read
description: Interrogate a client's Windows Servers through Liongard — installed roles, local admin members, services, patch level, OS version and EOL flags. Use for "what does this server do", local-admin audits, EOL-OS sweeps, or patch-posture checks per server.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard Windows Server Read

**When to use:** Triage lands on a server ("what roles/services does <device> run?"), "who's in local Administrators on <device> / across the fleet?", "any servers still on an end-of-life OS?", "when was <device> last patched?", or "what changed on this server?".

## Prompt

```
Read per-server Windows state for CLIENT_NAME from the Liongard Windows inspector. Read-only — never start/stop services or change group membership; service control via RMM is a separate gated workflow.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Windows" (one launchpoint per server). Verify last-run success per server and note the dataprint age — carry "as of <timestamp>" per server. Fleet sweeps iterate all Windows launchpoints — state coverage ("14 of 16 documented servers have working inspectors"); a fleet table mixing ages must show as-of per row or qualify the whole table.
2. Query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Identity/EOL: OS caption + build per server — flag versions at or past end of support, labeled "verify against Microsoft lifecycle dates" rather than asserting from memory.
   - Roles: installed roles/features — the "what does this box do" answer (AD DS, DNS, DHCP, IIS, Hyper-V, file services).
   - Local admins: Administrators group members per server — flag unexpected user accounts, per-user counts across the fleet (one account admin on every server is a finding), and non-standard service accounts. Findings are "unrecognized — verify" review inputs, never accusations; some entries are legitimate service accounts.
   - Services: services in a stopped state whose start mode is Automatic — the classic silent-breakage list; also new/unrecognized services.
   - Patch level: latest installed update date/build — servers >60 days behind get flagged. Patch posture from Liongard approximates; the RMM/patch platform is authoritative for compliance claims — say which source answered.
3. "What changed?" → liongard_detection / liongard_timeline per server: new local admin, service adds/state changes, role changes ordered against the incident window (time-adjacency, bounded by inspector cadence). An undocumented local-admin addition is a flag even with no incident.
4. Output: per-server or fleet table for the asked angle, flags with severity (EOL OS, unexpected admins, stopped-auto services, stale patching), source + dataprint age per server. Offer a plain-text add_ticket_note. Degradation: servers without inspectors → docs (search_itglue/search_hudu) → ticket history (search_tickets) → RMM.
```
