---
name: Bitdefender GravityZone
description: A Bitdefender GravityZone alert landed — identify which detection layer fired (on-access AV, behavioral ATC, HyperDetect, network attack, EDR incident), read Risk Analytics findings correctly, and use quarantine/rollback options without over- or under-trusting them.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# Bitdefender GravityZone

Vendor specialization of security-alert-response and edr-detection-runbook for Bitdefender GravityZone (the MSP multi-tenant console). GravityZone stacks several detection layers with different confidence characteristics; knowing which layer fired changes the triage. Verify module names and console layout against Bitdefender's current documentation — they evolve.

## When to use

- A GravityZone detection, EDR incident, or ransomware-mitigation alert arrives as a ticket
- A tech asks what an ATC or HyperDetect event means, or whether to restore a quarantined file
- A Risk Analytics report or risk-score item needs turning into desk work

## Steps

1. Identify the detection layer first — it sets the confidence baseline: on-access/on-demand antimalware (signature/cloud verdict, high confidence, usually auto-remediated), Advanced Threat Control (behavioral, mid-confidence, more false-positive-prone on niche LOB apps), HyperDetect (tunable pre-execution ML — its sensitivity level is a policy choice, so a HyperDetect hit at an aggressive setting is weaker evidence than a signature hit), Network Attack Defense (exploit/lateral-movement over the wire), and EDR incidents (correlated multi-event graphs — work these as incidents, not single alerts).
2. Parse the alert anatomy per security-vendor-generic: endpoint, user, detection name, file path/hash, and the action-taken field (disinfected, quarantined, blocked, reported-only). Route to the correct client per security-alert-response — the MSP console mixes tenants, so confirm the company/site field before assigning.
3. Containment matrix per edr-detection-runbook: auto-remediated + verifiable → verify in console, then scope calmly; reported-only or "detection" mode (common when a policy runs modules in report-only during rollout) → treat as live and contain first. GravityZone endpoint isolation, where licensed, is the containment tool; it is a technician action the agent directs and records.
4. Use the recovery options deliberately: quarantine restore is for confirmed false positives only — restoring requires the same evidence bar as dismissing an alert, plus an exclusion decision made properly (narrowest scope, named approver, review date). Ransomware Mitigation's file-restore (tamper-proof copies made at attack time) is a remediation aid after a confirmed ransomware verdict — branch to ransomware-response first; rollback does not replace scoping the intrusion.
5. Risk Analytics items (misconfigurations, vulnerable apps, human-risk behaviors) are posture findings, not incidents: batch them into remediation tickets per client with the risk score and affected endpoints, rather than treating each as an alert. Recurring noisy detections feed security-noise-tuning.
6. Document the decision, not just the action: layer that fired, verdict, evidence weighed, containment/restore decisions with approver, and classify per soc-classification-tree. Client-facing wording per defensive-writing-standard.

## Guardrails

- Never restore from quarantine on user say-so — "we need that file" is a business fact, not a benign verdict.
- A HyperDetect or ATC hit dismissed as false positive still needs the exclusion recorded and reviewed; silent per-endpoint exceptions rot.
- "Disinfected" covers the file, not the incident — scope-check persistence, siblings by hash, and identity exposure before closing.
- Report-only policy modes make the console look like it acted when it didn't — always read the action field, never assume remediation.
- The agent has no GravityZone console access: isolation, quarantine operations, and policy changes are technician steps the agent directs and records.
- Degradation: without documentation access, policy sensitivity settings are unknown — state that HyperDetect confidence can't be calibrated and lean on behavioral evidence.
