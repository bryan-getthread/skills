---
name: SentinelOne Threat Verdict
description: A SentinelOne threat detection needs triage — read the static vs behavioral engine verdict and mitigation status, direct kill/quarantine/rollback/disconnect decisions, and hold the line on exclusion requests.
category: Vendor Runbooks
tools: [search_tickets, search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_activities, get_ninjaone_device_link, add_ticket_note, update_ticket]
---

# SentinelOne Threat Verdict

Vendor specialization of edr-detection-runbook for SentinelOne. Adds the S1-specific reading of detection engines, mitigation states, and the rollback option — plus the exclusion-request discipline that keeps convenience from becoming an attack surface. Verify console layout and feature names against SentinelOne's current documentation.

## When to use

- A SentinelOne threat alert lands as a ticket (malicious/suspicious verdict, mitigation report)
- A tech asks whether to kill, quarantine, roll back, or disconnect a device in the S1 console
- A user or tech requests an exclusion because "S1 keeps blocking our app"

## Steps

1. Parse the S1 alert anatomy: threat name and file path/hash, the detecting engine — static AI (pre-execution, file-based) vs behavioral AI (it RAN and acted suspiciously) — the confidence level (malicious vs suspicious), the policy-driven mitigation already applied (kill/quarantine performed automatically under a "protect" policy vs alert-only under "detect"), and the mitigation status per action. A behavioral detection on an executed process is a different emergency than a static hit on a dormant file.
2. Device and user context per edr-detection-runbook: get_ninjaone_device for role and assigned user, get_ninjaone_device_activities around the detection time, and user corroboration via a verified channel — admin tools and installers are a large share of "suspicious" verdicts.
3. Read mitigation honestly: kill/quarantine reported complete → immediate execution stopped, but persistence and siblings are not ruled out; check the S1 storyline/process tree for what the process touched before dying. Detect-only or partial mitigation → treat as live.
4. Direct the console actions (technician executes in the S1 console; agent recommends and records):
   - Kill + quarantine — the default for a malicious verdict not yet fully mitigated.
   - Disconnect from network — for behavioral detections with signs of spread, C2, or hands-on-keyboard activity; do it before deep investigation.
   - Rollback (Windows, VSS-based) — for confirmed ransomware/file-modification damage; note it restores files, not credentials or persistence outside the storyline, and depends on intact Volume Shadow Copies.
   - Hand the technician get_ninjaone_device_link for hands-on remediation beyond the console.
5. Verdict assignment in S1 (true positive / false positive) follows the evidence bar of the generic runbook — a false-positive verdict in the console is a closure decision and needs the same corroboration as closing the ticket.
6. Exclusion requests get their own gate — see guardrails. A legitimate recurring false positive is a security-noise-tuning case: propose the narrowest possible exclusion (hash or signed-certificate scope preferred over path; path preferred over folder; never a whole drive or wildcard), with an owner and a review date.
7. Document the decision, not just the action; classify per soc-classification-tree; client-facing wording per defensive-writing-standard.

## Guardrails

- Exclusions are security decisions, not convenience — every exclusion is a permanent blind spot. Require: confirmed false-positive evidence, narrowest scope, named approver (security lead or management, not the requesting tech), and a documented review date. "It's annoying" is not a justification.
- Never mark false positive to silence noise — that trains the desk to miss the real one.
- Rollback is not absolution: after rollback, the initial access vector and any persistence still need remediation, or the encryption returns.
- A behavioral detection that executed stays open until the storyline scope is reviewed — "blocked" is not "done."
- Credential-touching tooling in the storyline → branch to compromised-account-containment for the signed-in user.
- Console actions are technician steps; the agent has no S1 access — direct, record timestamps, never assume completion.
- Degradation: no RMM connected → reduced device context; say so in the note.
