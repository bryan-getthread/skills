---
name: Cynet 360 AutoXDR
description: A Cynet detection landed — read its correlated XDR verdict and autopilot/automated-response state, decide what still needs a manual response action, and know when the Cynet MDR (CyOps) already handled it.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# Cynet 360 AutoXDR

Vendor specialization of security-alert-response and edr-detection-runbook for Cynet (Cynet 360 AutoXDR). Two traits shape triage: Cynet correlates endpoint, network, user, and deception signals into a single incident (XDR, not per-detection), and it leans heavily on automation — remediation playbooks and an autopilot mode that can act without a human, plus the CyOps MDR team who may respond on the client's behalf. The desk's job is to read the correlated verdict, verify what automation/CyOps already did, and work the remainder. The generic runbooks own the canon; this skill maps Cynet's model onto them. Verify feature names, autopilot behavior, and CyOps scope against Cynet's current documentation and the client's service tier.

## When to use

- A Cynet alert/incident arrives: malware/ransomware, suspicious behavior, user/login anomaly, network or deception (decoy) trigger
- A Cynet remediation playbook or autopilot action fired and the desk must validate
- A CyOps (Cynet MDR) analyst acted or notified, and follow-up is needed

## Steps

1. Work the incident, not the detection: Cynet groups correlated signals into one incident — read the whole correlation (endpoint + network + user + deception) before deciding class and tier. Copy Cynet's exact verdict/severity language into the note.
2. Parse the anatomy: affected host/identity, the correlated signals, the triggering process/file/hash, and — critically — the response state: was autopilot on (auto-remediated: kill/quarantine/isolate/disable) or was it alert-only, and did a CyOps analyst take action. Route to the client per security-alert-response by tenant/org; low confidence → flag for a human.
3. Verify automated/CyOps actions by effect (technician confirms in the Cynet console or the affected system): a claimed isolation, kill, or account disable is a claim until confirmed. Alert-only / autopilot-off → treat as live and contain first per edr-detection-runbook.
4. Investigate per edr-detection-runbook using the correlation: process lineage, persistence, what executed before any block, lateral movement across the correlated hosts. Deception/decoy triggers are high-signal — a hit on a decoy is rarely benign; treat as likely hands-on-keyboard. Identity involvement → compromised-account-containment; ransomware behavior → ransomware-response.
5. Coordinate with CyOps where they're engaged: split the timeline into CyOps actions (with their timestamps) vs desk actions (with yours), and agree on restoration/closure jointly so both sides consider it resolved.
6. Document the correlated verdict, response-state (autopilot/CyOps vs desk), evidence weighed, and classify per soc-classification-tree. False positives worth suppressing go through the exclusions guardrail. Client-facing wording per defensive-writing-standard.

## Guardrails

- Read the correlated incident before acting — a single Cynet detection is one node in an XDR story; tiering on it alone misjudges scope.
- Autopilot/CyOps actions are claims until verified by effect — never close on "Cynet remediated" without confirming the isolation/kill/disable actually holds.
- A deception/decoy trigger is treated as active adversary activity until proven otherwise — legitimate users don't touch decoys.
- Autopilot covers execution; persistence, initial access, and identity cleanup remain the desk's job — sweep before closing.
- Exclusions / autopilot-exception requests are security decisions: narrowest scope, named approver, review date.
- The agent has no Cynet console access — remediation, isolation lift, and verdict changes are technician steps the agent directs and records.
- Degradation: without documentation access, the client's Cynet tier (self-managed vs CyOps-MDR) and autopilot config may be unknown — say so rather than assuming who responds.
