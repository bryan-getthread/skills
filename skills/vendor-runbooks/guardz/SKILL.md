---
name: Guardz SMB Security
description: A Guardz alert landed — correlate its multi-signal SMB detections (email, identity, endpoint, external footprint) into one incident view rather than chasing each signal separately, and follow through on what auto-response staged.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# Guardz SMB Security

Vendor specialization of security-alert-response for Guardz, the MSP-focused unified security platform for small and mid-sized businesses. Guardz spans several detection surfaces at once — phishing/email, identity/dark-web exposure, endpoint posture, cloud-app and external-attack-surface — and its value is correlation across them. The triage discipline is to read a Guardz alert as *one signal in a possible multi-signal story*, check whether related signals exist for the same user/asset, and only then decide the incident's true class and tier. The generic runbooks own the canon; this skill maps Guardz's detection types onto them. Verify detection and auto-response names against Guardz's current documentation.

## When to use

- A Guardz alert arrives: phishing/email threat, compromised-credential / dark-web exposure, endpoint misconfiguration or malware, risky cloud-app or external-footprint finding
- A tech asks how to read a Guardz detection or whether several alerts are the same incident
- A Guardz automated response fired and the desk must validate and follow through

## Steps

1. Parse the anatomy per security-vendor-generic's five questions and identify the detection surface (email / identity / endpoint / cloud / external). Copy Guardz's exact wording. Route to the client per security-alert-response using tenant fields; low confidence → flag for a human.
2. Correlate before you commit to a class — Guardz's edge is breadth: search prior and concurrent context (search_tickets, same client + user/asset, recent window) for related Guardz signals. A dark-web credential hit plus a suspicious login for the same user is one identity incident, not two tickets; a phishing verdict plus an endpoint detection on the recipient is one intrusion.
3. Branch by the (correlated) class onto the matching generic runbook: identity/credential → compromised-account-containment; phishing → phishing-triage; endpoint → edr-detection-runbook; dark-web exposure → dark-web-alert-lifecycle; cloud/external posture → remediation planning.
4. Read auto-response honestly: treat any Guardz automated action as claimed-but-verify — confirm by effect before trusting it; detect-only findings are live.
5. Right-size the tier for SMB reality: many Guardz findings are posture/hygiene (weak config, exposed service) rather than active incidents — those are remediation items, not emergencies. Reserve incident response for evidence of active compromise, and don't inflate hygiene into incidents (or bury a real intrusion inside a hygiene digest).
6. Document the surface, the correlation result (what related signals were/weren't found), the class decision, auto-response verification, and classify per soc-classification-tree. Client-facing wording per defensive-writing-standard.

## Guardrails

- Correlate before classifying — Guardz's multi-signal design means a single alert may be one facet of a larger incident; treating facets as separate tickets fragments the response.
- Separate hygiene from incident: posture findings are remediation work, active-compromise evidence is incident response — mislabeling either is expensive.
- Auto-response actions are claims until verified by effect in the console.
- Dark-web / credential findings follow dark-web-alert-lifecycle — an exposed credential is only actionable tied to a still-valid account; verify before alarming the client.
- Exclusions / accepted-risk exceptions are security decisions: narrowest scope, named approver, review date.
- The agent has no Guardz console access — remediation, response changes, and containment are technician steps the agent directs and records.
- Degradation: without documentation access (search_itglue), the client's asset inventory and accepted-risk baseline may be unknown — say so.
