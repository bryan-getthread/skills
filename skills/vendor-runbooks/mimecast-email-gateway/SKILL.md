---
name: Mimecast Email Gateway
description: A Mimecast event needs working — a held message release request, a URL Protect click alert, or an impersonation-protect hit. Read the hold reason, apply release discipline, and treat allowed clicks as live incidents.
category: Vendor Runbooks
tools: [search_tickets, search_contacts, add_ticket_note, update_ticket]
---

# Mimecast Email Gateway

Vendor specialization of quarantine-release-request and phishing-triage for Mimecast's email gateway. The three recurring ticket shapes: held-message releases, URL Protect click events, and Targeted Threat Protection impersonation alerts. Verify policy names and console paths against Mimecast's current documentation.

## When to use

- A user asks to release a message Mimecast held, or replies to a held-message digest
- A URL Protect alert fires (a user clicked a rewritten link)
- An impersonation-protect alert lands (suspected CEO-fraud / lookalike sender)

## Steps

1. Held-message releases → run quarantine-release-request as the spine, with the Mimecast layer:
   - Read the hold reason: spam scanning, attachment-policy hold (blocked type or sandbox verdict), impersonation protect, or a content/administrative policy. The hold reason sets the scrutiny floor — a sandbox-malware or impersonation hold is never released on requester say-so.
   - Distinguish user-releasable holds (personal digest items the user could release themselves — ask why they didn't; it often means the hold class is admin-only for a reason) from admin-hold queues the technician works in the Mimecast console.
   - Release scope discipline: release to the requesting recipient only; "permit sender" is a separate allowlist decision with its own approver, not a rider on a release.
2. URL Protect click events → the pivotal field is the click outcome: blocked at click time vs allowed (scanned clean at click, or the user proceeded through a warning where policy permits). An allowed click on a later-confirmed-bad URL is a live event: run phishing-triage on the message, and if a credential page was involved treat it as credential exposure → compromised-account-containment for the clicker. For blocked clicks, confirm no sibling deliveries of the same URL to other users (technician checks the Mimecast logs; agent checks search_tickets for related reports).
3. Impersonation alerts (lookalike domain, display-name match, new-domain sender) → treat as business-email-compromise attempts: continue with vendor-fraud-bec-alert / typosquat-domain-alert logic. Never "release and warn" a held impersonation hit of a finance-flavored request; verify with the impersonated party via a number on file.
4. Recurring false positives on any path → security-noise-tuning: propose the narrowest permit (sender address over domain, domain over policy loosening) with a named approver and review date.
5. Document the decision, not just the action: hold reason or click outcome, requester verification, evidence, who released/permitted what and when. Console actions are technician steps; the agent recommends and records. Classify per soc-classification-tree; client-facing wording per defensive-writing-standard.

## Guardrails

- All quarantine-release-request guardrails apply: verify the requester, no payload interaction, urgency is a caution flag, when in doubt don't release.
- Never release a sandbox-malware or impersonation-verdict hold as a convenience — those verdicts exist to be inconvenient.
- An "allowed" URL Protect click is never informational — it means a user reached the destination; work it as exposure until the destination is confirmed benign.
- Rewritten Mimecast URLs in evidence notes: record the decoded original destination, and never click either form during assessment.
- "Permit sender" / allowlist changes are security decisions — separate approval, narrowest scope, review date.
- Degradation: without console access the agent sees only the ticket's copy of events; name what the tech should pull from the Mimecast logs (click logs, hold queue, policy match) rather than guessing.
