---
name: Liongard VMware Read
description: Interrogate a client's vCenter/ESXi estate through Liongard — host versions/build levels, datastore capacity, snapshot sprawl, VM inventory and placement. Use for "what's on their VMware", capacity questions, snapshot hygiene, or host-version currency during triage and QBR prep.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard VMware Read

**When to use:** "How full are <client>'s datastores?", "any old snapshots lying around?", "what ESXi versions/builds are their hosts on?", "what VMs do they have and where do they run?", or "what changed in the virtual estate?".

## Prompt

```
Read VMware estate state (vCenter/ESXi) for CLIENT_NAME from the Liongard VMware inspector. Read-only — never delete snapshots, migrate VMs, or change host config.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "VMware" (try "vCenter" / "ESXi"). Verify last-run success and note the dataprint age — carry "as of <timestamp>." A client may have a vCenter launchpoint plus standalone-host launchpoints — enumerate.
2. Query the angle via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Datastores: name, capacity, free — compute % free and flag <20% (warning) and <10% (urgent).
   - Snapshots: VMs with snapshots, age and size where present — flag snapshots older than 72h and any chain deeper than 2; snapshots on high-churn VMs (mail, SQL) get called out even younger. Backup software often owns transient snapshots — recommend "review for deletion," never "delete," and flag that consolidation on a near-full datastore needs free space.
   - Hosts: ESXi version + build per host — mixed builds within a cluster is a finding; report versions verbatim and recommend verifying support status against VMware/Broadcom lifecycle notices rather than asserting EOL from memory.
   - VM inventory: name, power state, host, datastore — the placement map; powered-off VMs consuming large disk are a cleanup lead.
3. "What changed?" → liongard_detection / liongard_timeline: VM adds/deletes, host changes, datastore changes ordered against the window. New-snapshot detections right before a space alert tell the whole story (time-adjacency, not proven cause).
4. Capacity numbers are as-of the last inspector run — on an active space incident, state the age and defer to live tooling for the current number. Output: capacity table, snapshot findings, host-version table, flags with severity, source + dataprint age. Offer a plain-text add_ticket_note. Degradation: inspector absent → docs (search_itglue/search_hudu) → ticket history (search_tickets).
```
