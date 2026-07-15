---
name: DLP Alert Triage
description: A data-loss-prevention alert fired — someone emailed, uploaded, or copied something the policy watches — separate business-process false positives from real exfiltration signals, investigating with respect for employee privacy.
category: Security
tools: [search_tickets, search_contacts, search_clients, add_ticket_note, update_ticket]
---

# DLP Alert Triage

Most DLP alerts are the business doing its job — payroll emailing payroll data, sales sending a contract. This skill works the alert on the evidence: what data, what destination, what pattern — reaching a verdict without turning the desk into a surveillance operation on the client's employees.

## When to use

- A DLP alert ticket arrives: sensitive-data pattern in outbound mail, cloud upload, USB copy, or share link
- A tech asks "is this DLP hit a real problem or just accounting doing accounting?"
- A cluster of DLP alerts on one user needs a careful, privacy-respectful look

## Steps

1. Extract the alert anatomy: policy/rule that matched, data classification and match count (e.g. "42 payment-card patterns"), the actor (user), the vector (email, cloud share, USB, print), the destination (recipient domain, share target, device), and the action taken by the platform (blocked, warned, logged-only). Route to the correct client per security-alert-response routing rules if on shared intake.
2. Business-context check — the false-positive test: does this flow match the actor's role and a routine process? Payroll → payroll processor's domain, monthly, similar volume = business as usual. Use search_contacts for the actor's role and search_tickets for the same rule + same flow history: a documented recurring benign pattern (same sender, same destination, same cadence) points to false positive — confirm it still applies, never auto-close on history alone.
3. Exfiltration-signal check — what makes it real:
   - Destination: personal email domains, personal cloud storage, or a competitor's domain — versus an established business counterparty.
   - Volume/spread: bulk or unusually broad data for that role; many files in a short window.
   - Timing and cluster: off-hours activity, or paired with other signals (resignation notice, access-revocation ticket in flight, recent security alerts on the account).
   - Behavior around controls: retrying after a block, renaming files or zipping to evade patterns.
   None alone is a verdict; accumulation raises the tier.
4. Privacy-respectful investigation discipline: examine only what the alert and verdict require — metadata (what pattern, where to, how much) before content; open the actual content only when the verdict genuinely requires it and the client's policy permits desk access to it, and record that it was accessed and why. Investigate the event, not the person's mailbox history at large. No verdicts about intent — describe what happened, not why you think they did it.
5. Verdict and path:
   - Business-process false positive → close with the evidence (role match, established destination, recurring pattern) and feed recurring ones to security-noise-tuning for a narrow policy exception (this sender-group to this destination — not "disable the rule").
   - Real exfiltration signals, or resignation/offboarding context → do NOT confront the user and do not investigate further solo: preserve the evidence and hand to the insider-risk-basics skill for the escalation-to-client-leadership path. Possible compromised account rather than insider (odd hours + other account anomalies) → security-alert-response / compromised-account-containment path.
   - Blocked-and-benign where the user just needs the legitimate path → answer the user's need (approved sharing method) so the block doesn't teach workarounds.
6. Document the decision, not just the action: anatomy, business-context findings, signals weighed, what content (if any) was accessed and under what authority, verdict, and route — plain-text note; classify per soc-classification-tree.

## Guardrails

- Employee privacy is a working constraint: metadata before content, content only with policy authority and a recorded reason, and never browsing beyond the event under investigation.
- No intent language in notes — "transferred 300 files to a personal account" is evidence; "trying to steal data" is a conclusion that belongs to the client's process, not the desk's note.
- Real-signal cases stop here and go to insider-risk-basics — no solo deep-dives, no user confrontation, no tipping anyone off.
- Recurring false positives get tuned narrowly at the DLP platform by its owner; never blanket-disable a rule, never PSA auto-close.
- A capped or partial alert history is reported as such; verdicts name their evidence.
- Degradation: without DLP-console access, work from the alert body and direct the technician on what to pull; record what could not be verified.
