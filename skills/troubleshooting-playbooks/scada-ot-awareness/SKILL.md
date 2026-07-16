---
name: SCADA / OT Awareness
description: Support an OT-adjacent ticket safely — knowing the hard boundary between IT and operational technology (SCADA/PLC/HMI/ICS), never touching controllers or industrial networks, and routing to the right OT/vendor owner. A safety-and-scope playbook, not a fix-it one. Cross-reference industry-packs/manufacturing.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# SCADA / OT Awareness

This playbook exists to keep an MSP tech *safe and in-scope* when a ticket brushes against operational technology — SCADA systems, PLCs, HMIs, industrial control networks, building-automation, medical/lab devices. The core skill is recognizing the boundary and stopping at it: OT controls physical processes where a wrong change can halt production, damage equipment, or endanger people. The right move is almost always to route to the OT/controls owner or the equipment vendor, not to troubleshoot. For vertical context, cross-reference industry-packs/manufacturing.

## When to use

- A ticket mentions SCADA, PLC, HMI, ICS, DCS, RTU, a controls/automation network, or "the plant/line/machine network"
- A device on an isolated/segmented "production" or "process" VLAN, or an air-gapped network, is involved
- An engineering workstation, historian, or HMI PC that talks to industrial controllers needs attention
- Building automation, medical devices, lab instruments, or other cyber-physical systems are in scope
- Anyone asks the MSP to "just get the machine/line back up" or change something on the control network

## Steps

1. **Recognize you're at the OT boundary — first and always.** Before any action, establish whether the ticket touches OT. search_itglue / search_hudu / search_knowledge_base for the client's OT documentation: is there a defined **IT/OT boundary**, a controls/automation owner (internal engineer or an integrator/OEM), documented "MSP may / may not touch" scope, and network segmentation between business IT and the process network? Many MSP contracts explicitly exclude OT — confirm the scope of engagement before proceeding. If OT scope is undocumented or unclear, treat everything past the boundary as out of scope until confirmed.
2. **History and ownership.** search_tickets for this client + the machine/line/OT: prior OT-adjacent tickets usually record who actually owns the controllers (the OEM, a systems integrator, an in-house controls engineer) and the agreed handoff. That owner, not the MSP, drives changes on the control side.
3. **Classify the request — which side of the boundary:**
   1. **Pure IT, merely near OT** — an engineering/HMI *Windows workstation's* OS, a historian's *server/database/backup*, business-network connectivity up to the segmentation point, standard user/account/print/email issues on the IT side. These are in normal MSP scope — but proceed with extra care because the machine may control or feed a live process (see guardrails).
   2. **The boundary itself** — the firewall/segmentation between IT and OT, data diodes, remote-access paths into OT. Read-and-report and coordinate; changes here are jointly owned with the OT owner and are never unilateral IT changes.
   3. **Pure OT** — the PLC/controller, HMI configuration/logic, the process/control network, drives, safety systems, industrial protocols (Modbus, Profinet, EtherNet/IP, DNP3, OPC). **Not MSP scope.** Do not connect to, scan, reconfigure, patch, or power-cycle these. Route to the OT/controls owner or OEM.
4. **Act only within the IT side; hand off the rest.** For the in-scope IT portion, apply the relevant standard playbook (Windows, backup, networking) *with OT-aware caution*: never reboot, patch, run a security scan, or change network settings on anything that might disrupt a live process without the OT owner's explicit coordination and a safe window (production may run 24/7). For anything OT, package what you know — symptom, what changed, the device/line, timing — and route it to the controls owner/OEM with a clear "this is OT, needs your action" note. When in doubt, do nothing and escalate.
5. **Document the boundary and the handoff.** Plain-text note: that the ticket is OT-adjacent, which side each part fell on (IT-in-scope vs OT-out-of-scope), what IT action was taken (if any) and with what coordination, and to whom the OT portion was routed. Recording the boundary decision protects the client and the MSP.

## Guardrails

- **Never touch controllers or the control network.** No connecting to, scanning (even a "harmless" ping sweep / vulnerability scan can crash fragile industrial devices), patching, reconfiguring, or power-cycling PLCs, HMIs, RTUs, drives, safety systems, or anything on the OT/process network. This is the non-negotiable rule.
- **Safety first, uptime second, always via the OT owner.** OT changes can cause physical harm, equipment damage, or production loss — decisions belong to the controls owner/OEM and the client's operations/safety authority, never the MSP acting alone.
- Confirm contractual scope before acting — many agreements exclude OT; if scope is unclear or undocumented, treat the OT side as out of scope and escalate rather than assume.
- Even IT-side work near OT is high-consequence — no reboots, patches, scans, or network changes on process-adjacent systems without the OT owner's coordination and a safe window; assume the process runs 24/7.
- No remote execution — this playbook is scope, safety, and routing; it does not fix OT. Standard RMM actions apply only to confirmed IT-side assets, with the caution above.
- Do not improvise from general IT knowledge on industrial systems or invent protocol/vendor specifics — OT is vendor- and integrator-specific; web_search only for general awareness and defer to the OEM's guidance and the controls owner.
- Cross-reference industry-packs/manufacturing for vertical context; keep client/site identifiers out of PSA notes. Notes destined for a PSA sync: plain text, no markdown or emojis.
