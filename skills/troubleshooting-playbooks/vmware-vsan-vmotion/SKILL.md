---
name: VMware vSAN and vMotion
description: Diagnose VMware vSphere problems — vSAN health warnings and resync storms, vMotion/DRS migration failures, datastore latency and APD/PDL — from vCenter health and event evidence, never by migrating or rebooting hosts blindly.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# VMware vSAN and vMotion

vSphere problems concentrate in storage (vSAN health, datastore latency, all-paths-down) and mobility (vMotion/DRS). This playbook reads vCenter's own health checks and events before anyone evacuates a host — because migrating VMs onto a stressed cluster, or rebooting a host mid-resync, can turn a warning into an outage.

## When to use

- vSAN Skyline Health shows warnings/errors, or the cluster is resyncing/rebuilding
- vMotion or Storage vMotion fails, or DRS won't balance / won't evacuate a host for maintenance
- Datastore latency spikes, VMs stunned or slow, or an APD/PDL (all-paths-down / permanent-device-loss) event
- A host is disconnected, not-responding, or won't enter maintenance mode

## Steps

1. **Version and topology first.** search_itglue / search_hudu / search_knowledge_base for the environment: vCenter/ESXi version and build, cluster size, storage model (vSAN vs traditional SAN/NFS), vSAN disk-group layout and dedup/compression, the vSAN and vMotion VMkernel networks, and the FTT/storage-policy in use. FTT (failures-to-tolerate) determines how many hosts can be down safely — establish it before touching a host. If a Liongard VMware inspector runs, read cluster/host state via liongard_launchpoint / liongard_metric and note the dataprint age.
2. **History first.** search_tickets for this client + VMware: a recent host patch/upgrade, a firmware/driver change (vSAN is exquisitely sensitive to storage-controller firmware/driver mismatches), a disk replacement, or a networking change. Sudden onset after maintenance points at what changed.
3. **Read health before acting.** vSAN: Skyline Health / cluster health checks, resync objects and ETA, disk-group and physical-disk state. vMotion/DRS: the exact task error and the DRS faults panel. Datastore: per-datastore and per-host latency (esxtop / performance charts), and any APD/PDL events in vmkernel.log. Read the actual failing check — don't act on "vSAN is unhealthy".
4. **Branch:**
   1. **vSAN health warnings / resync** — let Skyline Health name it: a failed/failing capacity or cache disk, a HCL/firmware-driver mismatch, a network problem (vSAN needs its VMkernel network healthy — MTU/jumbo-frame mismatches are classic), or object non-compliance. A resync in progress is the cluster self-healing — do not reboot or evacuate hosts during a resync unless you must, and never take a second host down while the cluster is already rebuilding from a first failure (that can breach FTT and lose data). Escalate when: it's a hardware/HCL/firmware issue — that's the hardware vendor and the storage owner.
   2. **vMotion / Storage vMotion failure** — read the error: vMotion-network connectivity/MTU, insufficient resources on the target, CPU/EVC incompatibility (mode mismatch across host generations), a device the VM can't migrate with (mounted ISO, passthrough/USB, affinity rule), or a stuck task. A failed vMotion normally leaves the VM running on the source — verify that before retrying. Storage vMotion failures are usually target-datastore space or latency.
   3. **DRS won't balance / host won't enter maintenance mode** — maintenance mode evacuation stalls when DRS can't place VMs elsewhere (resource shortfall, anti-affinity rules, a VM pinned by a device) or, on vSAN, when data-evacuation mode would breach availability. Read which VM is blocking and why. Don't force maintenance mode with "no data migration" on vSAN without understanding the availability impact.
   4. **Datastore latency / APD / PDL** — high latency points at the storage backend (array, HBA/NIC, fabric, or vSAN disk pressure); APD (paths temporarily gone) vs PDL (device permanently gone) are different recoveries — PDL usually needs the device removed/replaced and affected VMs handled deliberately. Check multipathing and the physical path per host. Escalate when: the fault is in the SAN/fabric/array — storage owner and vendor.
5. **Verify and note.** Success is vCenter's own report: Skyline Health green (or resync complete and objects compliant), a clean test vMotion, host connected and (if intended) in maintenance mode, datastore latency back to baseline. Plain-text note: version/build, FTT/storage policy, the health evidence, branch, action or handoff, verification.

## Guardrails

- No remote execution — all esxcli/vCenter/PowerCLI steps are guidance for a tech with vSphere access; use get_ninjaone_device_link only to reach an OS-level VM when relevant (hypervisor management isn't an RMM action).
- Never take a second host offline while the cluster is resyncing/rebuilding from a first failure — that can breach FTT and cause data loss; wait for compliance or escalate.
- Don't force-evacuate or reboot a host during a vSAN resync, and don't force maintenance mode with "ensure/no accessibility" options you don't understand — read the availability impact first.
- Datastore/SAN/fabric and firmware/HCL faults are the storage owner's and the hardware vendor's — package evidence, don't improvise on the array or push firmware from here.
- Do not invent error codes, esxcli syntax, or version behaviours — vSphere changes by version; web_search (VMware/Broadcom KB) and cite, and check the HCL for hardware/firmware questions.
- Docs/Liongard coverage varies per tenant — note what you couldn't check and dataprint age. Notes destined for a PSA sync: plain text, no markdown or emojis.
