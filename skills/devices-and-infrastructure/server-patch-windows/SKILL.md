---
name: Server Patch Windows
description: Plan and verify per-client server patching — map every server to its agreed maintenance window, sequence reboots correctly (dependencies first, domain controllers last and never together), and run the post-patch verification pass. Use for "when do <client>'s servers patch", planning a patch cycle, or verifying last night's window.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_activities, list_ninjaone_alerts, set_ninjaone_device_maintenance, get_ninjaone_device_link, connectwise_rmm_search_devices, connectwise_rmm_get_device, search_itglue, search_hudu, add_ticket_note, create_ticket, schedule_ticket]
---

# Server Patch Windows

Three deliverables around server patching: the window map (which server patches when, per client agreement), the reboot sequence (so dependencies come up in order and both domain controllers are never down together), and the morning-after verification pass (everything back, everything healthy).

## When to use

- "When are <client>'s servers patched?" / building or auditing the window map.
- Planning this month's patch cycle for a client with dependent workloads.
- The morning after a patch window: "did everything come back?"
- A server keeps missing its window and its patch posture is slipping.

## Steps

1. Build the window map: from documentation (`search_itglue` / `search_hudu`), the client's agreed maintenance windows and any blackout constraints (month-end for accounting clients, seasonal freezes); from the RMM (`search_ninjaone_devices` / `connectwise_rmm_search_devices`, verify server class in device details), the actual server list with patch status. Every server gets a row: window, patch policy, last patched, pending reboot age. Servers with no assigned window are finding number one.
2. Sequence the reboots inside each window by dependency, not alphabetically: infrastructure the others need comes back first (virtualization hosts before their guests — host reboots are `hypervisor-alerts` territory and often a separate window; storage/NAS before servers mounting it; database servers before the app servers that consume them). Domain controllers: never reboot all DCs simultaneously — patch them serially, verify each is back and advertising (logons/DNS answering) before the next, and schedule DCs last so everything else patches while authentication is still fully redundant. Where the true dependency order is undocumented, say the order is inferred and get it confirmed.
3. Pre-window prep: confirm backups for the in-scope servers completed recently (via the backup tool's status or `backup-failure-triage` findings — a failed backup the night before patching is a stop signal for that server), and suppress alert noise deliberately with `set_ninjaone_device_maintenance` for the window duration only.
4. During/after — the verification pass, per server: device back online in the RMM, uptime reflects the reboot (a server that "patched" without rebooting when a reboot was required has not finished), no new critical alerts (`list_ninjaone_alerts`), key services running, and patch activities show success not failure (`get_ninjaone_device_activities`). Then the service-level checks the RMM cannot see — application answering, shares mounted, line-of-business app logins — as a hands-on checklist handoff (`get_ninjaone_device_link`) or user confirmation in the morning.
5. Exceptions handling: failed patches, servers that missed the window, and pending reboots that never cleared each get a ticket (`create_ticket`) with the evidence, not a silent retry next month. End maintenance mode explicitly — expired-but-suppressed alerting is how the next real incident gets missed.
6. Output: the window map (or verification report), reboot sequence with rationale, per-server verification results (pass / fail / unverified), and exception tickets opened. Post the plain-text summary via `add_ticket_note`; book the next cycle's prep via `schedule_ticket`.

## Guardrails

- The RMM MCP cannot trigger patch deployment or run scripts — patch execution rides the RMM's own policies/schedules. This skill plans, sequences, gates, and verifies around them; never report patching as performed.
- Both/all domain controllers down at once is never acceptable — serial DC patching with verification between is non-negotiable in every sequence you produce.
- No patching a server whose latest backup failed or is stale, unless the client explicitly accepts the risk in writing.
- Maintenance mode is set for the window and lifted after verification — never left open-ended.
- "Patched" claims come from patch activity evidence, not from the schedule having elapsed. Report unverified servers as unverified.
- Dependency order presented without documentation backing is labeled as inferred.
- If the RMM integration is absent, produce the window map and sequence from documentation and hand the whole execution to the patching tool's operator; do not fabricate patch status.
- Notes are plain text — no markdown, no emojis.

## See also

- `patch-compliance-review` — posture across the fleet; this skill owns the scheduling/sequencing/verification of the window itself.
- `maintenance-mode-workflow` — the alert-suppression discipline used in step 3.
- `hypervisor-alerts` — host-level patching and its guest choreography.
