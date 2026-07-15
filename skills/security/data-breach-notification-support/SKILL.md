---
name: Data Breach Notification Support
description: An exposure of client data is confirmed and notification obligations may apply — preserve the evidence, build the factual timeline, and support the counsel-led notification process by drafting facts, never legal conclusions.
category: Security
tools: [search_tickets, add_ticket_note, update_ticket, search_itglue, view_openDraft]
---

# Data Breach Notification Support

When an incident crosses into confirmed data exposure, the desk's role narrows and sharpens: keep the evidence intact, produce the timeline every downstream party will ask for, and give counsel accurate raw material. Whether it legally constitutes a "breach," who must be notified, and by when are determinations for lawyers — the desk supplies the facts that make those determinations possible.

## When to use

- An investigation confirms client data was accessed, exfiltrated, or exposed (mailbox rifled by an attacker, database left open, files taken in a ransomware event)
- The client's counsel, insurer, or IR firm requests a timeline, evidence, or notification support
- A client asks "do we have to report this?" — the answer is this process, not an opinion

## Steps

1. Confirm the lane immediately: notification decisions are counsel-led. On confirmed exposure, the desk's management engages the client's incident policy chain (search_itglue for their IR plan, counsel, and cyber-insurance carrier — the carrier often supplies counsel). The desk does not decide whether this is a reportable breach, does not pick jurisdictions or deadlines, and does not notify anyone on its own initiative.
2. Preserve evidence before it degrades:
   - Freeze relevant logs against retention aging (sign-in logs, audit logs, mail-trace, access logs on the exposed system) — direct the technician to export/hold them now; several key log sources have short default retention windows.
   - Preserve system state: isolated hosts stay preserved (per ransomware-response), attacker artifacts (rules, OAuth consents, created accounts) are documented before removal — removal itself is timestamped.
   - Chain of custody discipline: record who exported what, from where, when, and where it is stored; store evidence per counsel's instruction once counsel is engaged.
3. Build the factual timeline — the core deliverable: a chronological, timestamped (timezone-stated) record from earliest known attacker activity through detection, containment steps (pull from the containment notes — this is why those runbooks timestamp everything), investigation findings, and recovery. Every line traces to a source: log entry, ticket note, tool report. Gaps are stated as gaps ("no visibility between X and Y — log source not enabled"), never smoothed over.
4. Scope facts for counsel's data-analysis questions, stating confidence honestly: what data categories the affected systems held, what the logs show was accessed versus what merely *could* have been accessed (this distinction drives everything downstream — never blur it), how many records/individuals potentially involved (with the basis for the count), and which clients'-customers' or employees' data is implicated. "Unknown" and "pending log analysis" are correct answers; guesses are not.
5. Support notification drafting when counsel directs: the agent drafts factual components — what happened in defensive-writing-standard language, dates, data categories, remediation steps taken — for counsel to review, edit, and approve. Counsel owns: whether to notify, whom, deadlines, regulatory filings, and all legal characterizations. Nothing drafted here is sent by the desk; drafts go to counsel via the client's designated channel (view_openDraft for human handling).
6. Keep the record running: every counsel/insurer request and what was provided, every preservation action, timestamped in restricted plain-text notes. Post-notification, feed the whole arc to security-incident-postmortem — including how well evidence and timeline held up, which is the best test of the desk's IR hygiene.

## Guardrails

- Facts, never legal conclusions: the desk's output never states "this is/is not a reportable breach," never names notification deadlines or applicable laws, and never promises "no data was accessed" without log evidence that affirmatively shows it.
- Accessed vs. could-have-been-accessed stays explicit in every summary; conflating them misleads counsel in both directions.
- Nothing is deleted, "cleaned up," or remediated-over before preservation is confirmed — and attacker-artifact removal is itself evidence, timestamped.
- No outbound notification of any kind (client's customers, regulators, media) originates from the desk; drafts exist only for counsel's review.
- Defensive-writing-standard governs every word — "incident" and "unauthorized access to X" per evidence; "breach" only where counsel has adopted it.
- Timeline honesty: visibility gaps are disclosed, confidence levels stated, nothing back-filled by inference without labeling it as inference.
- Degradation: without search_itglue, obtain the client's counsel/carrier chain through management before producing anything beyond preservation.
