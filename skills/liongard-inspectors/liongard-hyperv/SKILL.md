---
name: Liongard Hyper-V Read
description: Interrogate a client's Hyper-V hosts through Liongard — host and VM inventory, checkpoint sprawl, replica health, VM placement and state. Use for "what runs on their Hyper-V", checkpoint hygiene, replication-status checks, or virtual-estate context during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Hyper-V Read

Reads Hyper-V estate state — hosts, VMs, checkpoints, replication — from Liongard inspector data. Hyper-V's equivalents of VMware's classic problems show up here: checkpoint (snapshot) sprawl silently filling volumes, and Hyper-V Replica quietly unhealthy until the day it's needed.

## When to use

- "What VMs run on <client>'s Hyper-V host(s)?" — inventory for triage, DR planning, or migration scoping.
- "Any old checkpoints?" — checkpoint sprawl is the top silent disk-eater on Hyper-V hosts.
- "Is their Hyper-V replication healthy?" — replica-state check before trusting the DR story.
- Host disk-space incident: checkpoint + VM placement picture first.
- "What changed on the host?" after VM or host issues.

## What the inspector captures

Host identity and OS/Hyper-V version, VM inventory (name, state, generation, resources, storage paths), checkpoints per VM with age where present, and replication configuration/health for replica-enabled VMs. Hyper-V state may also surface partly through the Windows Server inspector on the host — coverage varies by inspector and version; confirm what the launchpoint actually captures per the access pattern.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve environment, `liongard_launchpoint` with systemType "Hyper-V" — if none, check for a Windows Server launchpoint on the known host, whose dataprint may carry the Hyper-V role data. Verify last-run success, note dataprint age. Clusters: one launchpoint per node is common — enumerate nodes before summing VMs (clustered VMs appear on their current owner).
2. Query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - VM inventory: name, state, host, storage path — the placement map; VHDX paths on the system volume are a flag on their own.
   - Checkpoints: VMs with checkpoints + age — flag >72h old and any deep chains; note that backup products create transient checkpoints, so a young checkpoint during the backup window is normal.
   - Replication: replica-enabled VMs with health state — anything not "Normal" is a finding; also flag VMs the client believes are replicated but show no replica config.
   - Host: OS version + Hyper-V role — feed EOL-OS concerns to liongard-inspectors/liongard-windows-server.
3. "What changed": `liongard_detection` / `liongard_timeline` — VM adds/removals, checkpoint creation, replication-state transitions ordered against the incident window.
4. Output: inventory table, checkpoint findings, replication-health table, flags with severity, source + dataprint age. Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: host disk-space or VM-performance tickets open with the checkpoint + placement picture attached.
- **Incident correlation**: checkpoint-creation detections before a space alert; replication-health transitions before a failed DR test. Time-adjacency, not proven cause.
- **QBR / DR evidence**: replica-health status and checkpoint-hygiene stats are DR-readiness lines; VM counts and host versions feed capacity and refresh planning.

## Guardrails

- Read-only: never delete checkpoints, alter replication, or move VMs. Checkpoint deletion merges disks and needs free space + IO headroom — flag that when recommending cleanup on a stressed host.
- Replication "Normal" in a day-old dataprint is not "healthy right now" — state the age, and on DR-critical questions recommend a live check.
- Distinguish backup-created transient checkpoints from forgotten manual ones before flagging; when unsure, say "review — may be backup-managed".
- Clustered environments: state which nodes you covered; a node without a working inspector means an incomplete VM census — say so.
- Degrade per the access pattern (docs → ticket history) when no inspector covers the host.
- Plain-text notes only.
