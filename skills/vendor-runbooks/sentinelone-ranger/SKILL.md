---
name: SentinelOne Ranger
description: A SentinelOne Ranger network-discovery finding landed — read the rogue/unmanaged-device signal, tell a genuinely unknown asset from known-but-unmanaged infrastructure, and drive it to identify-then-manage without blind network action.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_link, add_ticket_note, update_ticket]
---

# SentinelOne Ranger

Specialization of sentinelone-threat-verdict for SentinelOne Ranger — the network-discovery / device-inventory capability, as distinct from endpoint threat detection. The parent skill owns S1 detection-engine reading, mitigation, and exclusion discipline; this skill covers Ranger's different output: it uses already-deployed S1 agents to passively map the network and surface devices that have *no* S1 agent — rogue or unmanaged assets. The triage discipline is identification before action: a "rogue device" is usually a known printer, IoT sensor, switch, or an EDR-coverage gap on a real workstation — not an intruder. Read sentinelone-threat-verdict first; do not duplicate its threat-verdict steps. Verify Ranger feature names against SentinelOne's current documentation.

## When to use

- A Ranger finding reports a new/unmanaged/rogue device on a client's network
- A coverage-gap question arises ("which endpoints have no S1 agent?")
- A tech asks how to read a Ranger discovery or whether a discovered device is a threat

## Steps

1. Parse the Ranger finding: discovered device's IP/MAC, hostname if resolved, OS/fingerprint guess, the network/subnet and the S1 agent that observed it, first-seen time, and whether Ranger classifies it managed/unmanaged/unknown. Copy Ranger's exact wording. Route to the client per security-alert-response using the observing site/tenant; low confidence → flag for a human.
2. Identify before alarming — most "rogue" devices are mundane. Cross-reference the RMM inventory (search_ninjaone_devices by hostname/MAC/IP; get_ninjaone_device for a match) and documentation to answer: is this a known asset that simply lacks an S1 agent (coverage gap), a known non-endpoint device that *can't* run S1 (printer, switch, IoT, phone), or a genuinely unrecognized device? The MAC OUI (vendor prefix) is a strong first clue to device type.
3. Classify the finding into the right lane:
   - Known asset, no S1 agent → EDR coverage gap: an install/deployment task (technician action), not an incident. Note it for coverage remediation.
   - Known non-endpoint device → expected; record and, if recurring, suppress via scoped tuning so it stops surfacing as "rogue."
   - Genuinely unidentified device on a trusted segment → treat as a security question per security-alert-response: who plugged in what, when; correlate with any access-control/DHCP/switch logs (technician-gathered) before concluding.
4. Do not take blind network action: Ranger can, with configuration, help block/quarantine unknown devices at the network layer — but blocking an unidentified device that turns out to be a critical printer or medical/OT device causes its own outage. Identify first; any network-level containment is a deliberate, technician-executed, documented decision.
5. Hand off: agent installs, network-block decisions, and switch/DHCP investigation are technician actions — provide get_ninjaone_device_link for a matched managed device, and a clear handoff for the rest. The agent directs and records.
6. Document the identification result, the lane, coverage-gap items, and any containment decision with its owner. Client-facing wording per defensive-writing-standard.

## Guardrails

- Identify before you act — most Ranger "rogue" devices are printers, IoT, network gear, or coverage gaps; treating every one as an intruder burns trust and risks outages.
- Never blind-block an unidentified device at the network layer — an OT/medical/printer misfire is its own incident; network containment is a deliberate, documented, technician-executed decision.
- A coverage gap is a deployment task, not a threat — but a genuinely unknown device on a trusted segment is a security question; don't collapse the two.
- Agent deployment, network blocking, and switch/DHCP forensics are technician steps — the agent has no S1/Ranger or network-device console access; direct and record, never assume completion.
- Recurring known-benign devices get scoped suppression, not blanket disabling of discovery.
- Do not duplicate sentinelone-threat-verdict — for an actual endpoint threat verdict, use that skill; this one is the network-discovery layer.
- Degradation: without RMM (search_ninjaone_devices) or documentation, identification is limited to IP/MAC/OUI — say so and lean on technician-gathered network evidence.
