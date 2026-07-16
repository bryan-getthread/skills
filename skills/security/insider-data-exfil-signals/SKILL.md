---
name: Insider Data Exfil Signals
description: A ticket carries signs of a user exfiltrating data — mass downloads, external forwarding of their own files, USB copying, cloud-sync to personal accounts, often around a departure — recognize it, preserve quietly, and escalate; never investigate solo.
category: Security
tools: [search_tickets, add_ticket_note, update_ticket, search_itglue]
---

# Insider Data Exfil Signals

The hardest security signal to handle, because the "attacker" is an authenticated, authorized user and the wrong move tips them off or oversteps HR/legal boundaries. This skill extends insider-risk-basics for the specific case of data exfiltration: recognize the pattern, preserve evidence without alerting the subject, and escalate into the client's HR/legal channel. It is a recognition-and-handoff skill, not an investigation.

## When to use

- Signals of a user moving data out: bulk file downloads/exports, forwarding their own mailbox or files to a personal address, large USB/removable-media transfers, syncing company data to personal cloud storage, mass access to files outside their normal scope
- The signals cluster around a resignation, termination, or dispute (the highest-risk window)
- Another runbook (offboarding, DLP alert, lost-device) surfaces behavior that looks like intentional exfiltration

## Steps

1. Recognize the pattern, don't chase it: note the specific signals and their timing. Departure + spike in data movement is the classic combination — but timing is a flag, not a verdict.
2. STOP before investigating: this is the insider-risk-basics rule and it is absolute here — do NOT open your own investigation, do NOT confront the user, do NOT disable their access on your own initiative, and do NOT discuss it beyond the authorized channel. Premature action can destroy evidence, tip the subject, or create employment-law liability for the client and the MSP.
3. Preserve quietly: capture the evidence that already exists (DLP/alert logs, sign-in and file-access logs, forwarding rules, timestamps) into a confidential record without taking actions the subject would notice. Use existing telemetry — don't provision new surveillance of a specific employee without client legal authorization.
4. Escalate to the right owner: route to the client's designated leadership/HR/legal contact per the client's insider-risk or incident policy, not to the user's manager casually and not broadly. If no policy/contact is documented, escalate to the MSP's security lead to establish the authorized channel first.
5. Take protective action only on explicit authorized instruction: any access change, mailbox hold, or containment happens only when the client's authorized owner directs it — often coordinated with employee-offboarding. Record who authorized what.
6. Keep it confidential and minimal: restrict ticket visibility per policy, keep the subject's name and specifics on a need-to-know basis, and avoid speculation about intent in writing.
7. Document facts only: the observed signals with timestamps, the evidence preserved, who it was escalated to and when, and any authorized actions. No conclusions about guilt or intent.

## Guardrails

- DO NOT investigate solo — the defining rule. Recognize, preserve, escalate. Confrontation, self-directed access changes, and freelance investigation are all off-limits.
- This is HR/legal territory before it is IT territory — the client's authorized owner drives; the agent supports on instruction. Overstepping creates employment-law and privacy liability.
- Timing near a departure is a flag, not proof — legitimate work generates data movement too. Preserve and escalate; do not accuse.
- Don't stand up targeted surveillance of a named employee without client legal authorization — work from telemetry that already exists.
- Confidentiality is part of the control — restrict visibility, minimize who knows, keep intent-speculation out of writing.
- Defensive writing and no fabrication: record observed signals, never characterize the person or their motives.
- Plain text only for PSA-synced notes.
- When in doubt, preserve and escalate — under-reacting-into-the-right-channel is recoverable; a tipped-off subject or an overstep is not.
