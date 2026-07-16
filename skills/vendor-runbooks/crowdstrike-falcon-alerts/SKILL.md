---
name: CrowdStrike Falcon Alerts
description: A CrowdStrike Falcon detection or incident landed — parse the detection anatomy, decide whether Network Contain is warranted, and know when mass simultaneous endpoint failures mean a vendor-side problem rather than an attack.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# CrowdStrike Falcon Alerts

Vendor specialization of security-alert-response and edr-detection-runbook for CrowdStrike Falcon. The generic runbooks own the investigation canon; this skill adds Falcon's detection/incident packaging, its containment semantics, and the lesson of July 2024: sometimes the agent itself is the outage. Verify feature names and console layout against CrowdStrike's current documentation — they evolve.

## When to use

- A Falcon detection or incident alert arrives as a ticket (email connector, SIEM forward, or MDR escalation)
- A tech asks whether to network-contain a host or how to read a Falcon verdict
- Many endpoints across one or more clients fail simultaneously and someone suspects the sensor or a content update

## Steps

1. Parse the Falcon alert anatomy: detection vs incident (an incident groups related detections — work the incident, not each detection in isolation), severity (Informational through Critical), tactic/technique mapping, hostname and sensor ID, user context, the process tree or triggering file/hash, and — critically — the action-taken field (blocked/killed/quarantined vs detection-only). Copy Falcon's exact verdict language into the ticket note.
2. Route to the correct client per security-alert-response using hostname/tenant (CID) fields — Falcon alerts often arrive on a shared intake mailbox. Low routing confidence → no reassignment, flag for a human.
3. Containment decision per the edr-detection-runbook matrix, with Falcon semantics: Network Contain isolates the host from the network but keeps the Falcon cloud channel open — the tech can still investigate and lift containment remotely. That makes containment cheap to apply and cheap to undo: on detect-only alerts for credential theft, hands-on-keyboard indicators, or lateral-movement tooling, recommend containing first and investigating second. Containment is a technician action in the Falcon console; the agent directs and records it.
4. Investigate per edr-detection-runbook: process lineage, persistence, what executed before blocking, other hosts with the same hash/indicator (search_tickets for prior context, same client and indicator, 90 days). Confirmed-malicious with identity involvement → branch to compromised-account-containment; ransomware behavior → ransomware-response.
5. Vendor-caused mass-failure check — the 2024 lesson: on 19 July 2024 a faulty Falcon content update caused mass Windows BSOD boot loops worldwide; it was not an attack. If many hosts fail, BSOD, or go offline simultaneously across unrelated clients, check CrowdStrike's status page and advisories and the desk's other monitoring BEFORE working it as a security incident. Symptoms clustered tightly in time, spanning clients, with no preceding detections point vendor-side. If vendor-caused: switch posture to outage response — open a tracking ticket per affected client, follow the vendor's official remediation bulletin exactly (do not improvise boot-loop fixes from memory), and communicate per defensive-writing-standard without speculating about cause until the vendor confirms.
6. Document the decision, not just the action: severity and verdict, containment state and who applied/lifted it, evidence weighed, and classify per soc-classification-tree. False positives worth suppressing go through the exclusions guardrail, never silently.

## Guardrails

- "Blocked" in a Falcon detection is a claim about one process — verify nothing executed before the block and scope-check the host before closing.
- Never lift Network Contain without a documented verdict and a named decision-maker.
- Exclusions and IOC allowlisting are security decisions: narrowest scope (hash over path, path over folder), named approver, review date.
- Mass simultaneous failures across clients: rule out a vendor/content-update cause before declaring a security incident — and rule out an attack before declaring it vendor-side; both mistakes are expensive.
- The agent has no Falcon console access: containment, detection status changes, and RTR sessions are technician steps the agent directs and records.
- Client-facing wording per defensive-writing-standard — "a detection was investigated and contained," not "you were hacked."
- Degradation: without documentation access (search_itglue), sensor deployment scope may be unknown — say so rather than assuming full coverage.
