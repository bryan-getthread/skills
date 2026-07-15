---
name: Huntress EDR Incident
description: A Huntress EDR incident report arrived — foothold, persistence, or active threat on an endpoint. Read what Huntress already did (isolation, remediation steps), execute what remains, and verify before closing.
category: Vendor Runbooks
tools: [search_tickets, search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_activities, get_ninjaone_device_link, add_ticket_note, update_ticket]
---

# Huntress EDR Incident

Vendor specialization of edr-detection-runbook for Huntress managed EDR. Huntress is analyst-reviewed: an incident report means a human SOC analyst already judged it real, which changes the default posture from "is this noise?" to "what remains to be done?" Verify report structure and portal actions against Huntress's current documentation.

## When to use

- A Huntress incident report lands as a ticket (foothold, persistence mechanism, malicious process, ransomware canary)
- A Huntress-isolated host needs follow-up and release
- A tech asks what to do with the "assisted remediation" steps in a Huntress report

## Steps

1. Parse the incident report anatomy: severity (Low/High/Critical), affected hostname, the finding class — footholds/persistence (autoruns, scheduled tasks, services, registry run keys) are Huntress's signature detection — indicators (paths, hashes, command lines), and the two action sections: what Huntress already executed (automated remediation, host isolation if enabled for the org) and the remediation steps awaiting the technician (approve-to-run remediations and fully manual steps).
2. Trust the verdict, verify the scope: because a Huntress analyst reviewed it, do not burn time re-litigating "is it real?" — spend it on scope instead. Pull device context (get_ninjaone_device, get_ninjaone_device_activities) for role (server detections are higher stakes), assigned user, and activity around the detection time.
3. Isolation status check: the report states whether the host was isolated. If isolated → the immediate spread risk is handled; plan remediation before release. If NOT isolated on a High/Critical incident → isolating it is the first technician action, before investigation continues.
4. Execute the remainder: approve pending Huntress remediations in the portal (technician action), then work the manual steps in order. Hand the technician get_ninjaone_device_link for hands-on work — the agent cannot run scripts or deploy software through the RMM.
5. Credential blast-radius: if the finding class implies credential access (infostealer, credential-dumping tooling, RDP foothold), branch to compromised-account-containment for the signed-in user(s) — Huntress's endpoint remediation does not reset identities.
6. Verify before release/close: remediation steps marked complete in the Huntress portal, no new detections on the host, persistence locations rechecked. Release isolation only after that verification, and record who released it and when.
7. Document the decision, not just the action — Huntress actions vs technician actions, timestamps, verification evidence — and classify per soc-classification-tree. Client-facing wording follows the defensive-writing-standard.

## Guardrails

- "Huntress remediated" covers the listed items only — footholds come in sets; confirm the report's full checklist is complete before closing.
- Never release host isolation to stop user complaints — the inconvenience is the containment working. Release requires completed remediation plus verification.
- Never close on the automated actions alone when manual steps remain unchecked in the report.
- Anything suggesting lateral movement or multiple hosts → escalate to the incident path, not per-ticket working; check for sibling reports on other devices at the same client.
- Portal actions (approve remediation, release isolation, close incident) are technician steps — the agent directs, records, and never assumes they happened without confirmation.
- Degradation: no RMM connected → work from the report body and ticket history; state the reduced device visibility in the note.
