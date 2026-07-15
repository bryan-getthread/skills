---
name: Liongard VMware Read
description: Interrogate a client's vCenter/ESXi estate through Liongard — host versions/build levels, datastore capacity, snapshot sprawl, VM inventory and placement. Use for "what's on their VMware", capacity questions, snapshot hygiene, or host-version currency during triage and QBR prep.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard VMware Read

Reads VMware estate state — ESXi hosts and builds, clusters, datastores, VMs, snapshots — from the Liongard VMware (vCenter/ESXi) inspector. The recurring wins: datastore-capacity answers before a "server is slow" spiral, snapshot sprawl before it eats a datastore, and host-version facts for upgrade/advisory planning.

## When to use

- "How full are <client>'s datastores?" — capacity check during a slowness/space incident or planning pass.
- "Any old snapshots lying around?" — snapshot-sprawl hygiene (a top cause of sudden datastore exhaustion).
- "What ESXi versions/builds are their hosts on?" — advisory or upgrade planning.
- "What VMs do they have and where do they run?" — inventory for triage, DR planning, or a migration quote.
- After a host or vCenter change: "what changed in the virtual estate?"

## What the inspector captures

vCenter/host inventory (ESXi version and build per host, cluster membership), datastore list with capacity/free space, VM inventory (name, power state, host/datastore placement, resources), and snapshot data per VM. Standalone ESXi vs vCenter-managed estates differ in depth; coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve environment, `liongard_launchpoint` with systemType "VMware" (try "vCenter"/"ESXi" variants), verify last-run success, note dataprint age. A client may have a vCenter launchpoint plus standalone-host launchpoints — enumerate.
2. Query the angle via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Datastores: name, capacity, free — compute % free and flag <20% (warning) and <10% (urgent).
   - Snapshots: VMs with snapshots, snapshot age and size where present — flag snapshots older than 72h and any chain deeper than 2; snapshots on high-churn VMs (mail, SQL) get called out even younger.
   - Hosts: ESXi version + build per host — mixed builds within a cluster is a finding; report versions verbatim and recommend verifying support status against VMware/Broadcom lifecycle notices rather than asserting EOL from memory.
   - VM inventory: name, power state, host, datastore — the placement map; powered-off VMs consuming large disk are a cleanup lead.
3. "What changed": `liongard_detection` / `liongard_timeline` — VM adds/deletes, host changes, datastore changes ordered against the window. New-snapshot detections right before a space alert tell the whole story.
4. Output: capacity table, snapshot findings, host-version table, flags with severity, source + dataprint age. Offer a plain-text `add_ticket_note`; capacity findings can also feed devices-and-infrastructure/storage-capacity-planning.

## How this feeds tickets

- **Triage**: "server slow / disk full" tickets open with the datastore + snapshot picture attached — often the diagnosis.
- **Incident correlation**: snapshot creation or VM-add detections vs the space-alert window; host changes vs VM-availability incidents. Time-adjacency, not proven cause.
- **QBR / capacity planning**: datastore trends, host build currency, and VM counts are QBR staples; snapshot-hygiene stats make a good managed-services value line.

## Guardrails

- Read-only: never delete snapshots, migrate VMs, or change host config — those are tech-owned with change control. Snapshot deletion on a near-full datastore is especially dangerous (consolidation needs free space) — flag that explicitly when recommending cleanup.
- Capacity numbers are as-of the last inspector run — on an active space incident, state the age and defer to live tooling for the current number.
- Snapshot findings name the VM and age, and recommend "review for deletion", never "delete" — backup software often owns transient snapshots.
- Hypervisor-alert response (host down, HA events) belongs to devices-and-infrastructure/hypervisor-alerts; this skill supplies posture context to it.
- Degrade per the access pattern (docs → ticket history) when the inspector is absent.
- Plain-text notes only.
