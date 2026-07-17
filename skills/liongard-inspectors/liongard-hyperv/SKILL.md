---
name: Liongard Hyper-V Read
description: Interrogate a client's Hyper-V hosts through Liongard — host and VM inventory, checkpoint sprawl, replica health, VM placement and state. Use for "what runs on their Hyper-V", checkpoint hygiene, replication-status checks, or virtual-estate context during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard Hyper-V Read

**When to use:** "What VMs run on <client>'s Hyper-V host(s)?", "any old checkpoints?" (the top silent disk-eater), "is their Hyper-V replication healthy?", a host disk-space incident, or "what changed on the host?".

## Prompt

```
Read Hyper-V estate state for CLIENT_NAME from the Liongard inspector. Read-only — never delete checkpoints, alter replication, or move VMs.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Hyper-V" — if none, check for a Windows Server launchpoint on the known host, whose dataprint may carry the Hyper-V role data. Verify last-run success and note the dataprint age — carry "as of <timestamp>." Clusters: one launchpoint per node is common — enumerate nodes before summing VMs (clustered VMs appear on their current owner); state which nodes you covered.
2. Query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - VM inventory: name, state, host, storage path — the placement map; VHDX paths on the system volume are a flag on their own.
   - Checkpoints: VMs with checkpoints + age — flag >72h old and any deep chains; backup products create transient checkpoints, so a young checkpoint during the backup window is normal — when unsure, say "review — may be backup-managed." Checkpoint deletion merges disks and needs free space + IO headroom — flag that when recommending cleanup on a stressed host.
   - Replication: replica-enabled VMs with health state — anything not "Normal" is a finding; also flag VMs the client believes are replicated but show no replica config. "Normal" in a day-old dataprint is not "healthy right now" — on DR-critical questions recommend a live check.
   - Host: OS version + Hyper-V role — feed EOL-OS concerns to the Windows Server read.
3. "What changed?" → liongard_detection / liongard_timeline: VM adds/removals, checkpoint creation, replication-state transitions ordered against the incident window (time-adjacency, not proven cause).
4. Output: inventory table, checkpoint findings, replication-health table, flags with severity, source + dataprint age. Offer a plain-text add_ticket_note. Degradation: no inspector covers the host → docs (search_itglue/search_hudu) → ticket history (search_tickets).
```
