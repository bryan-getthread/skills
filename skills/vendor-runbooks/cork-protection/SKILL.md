---
name: Cork Protection Posture
description: A Cork cyber-warranty posture signal landed — read which required control slipped, understand that the signal is a warranty-eligibility gap (not an active incident), and drive it back into compliance before coverage lapses.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_itglue, add_ticket_note, update_ticket]
---

# Cork Protection Posture

Vendor specialization for Cork, the MSP cyber-warranty platform. Cork monitors whether a client's stack maintains the security controls its warranty requires (EDR deployed and healthy, MFA enforced, backup running, email security active, etc.) and signals when a control slips out of compliance. The crucial framing: a Cork posture signal is usually *not* an active security incident — it's a warranty-eligibility gap that, left unaddressed, can void coverage or reduce a future payout. Triage it as a compliance-remediation item with a business/insurance clock, escalating to the security path only when the slipped control also evidences a live threat. Verify control names, warranty terms, and monitored signals against Cork's current documentation and the client's specific warranty.

## When to use

- A Cork signal fires that a required control is missing, unhealthy, or out of compliance (EDR gap, MFA not enforced, backup failing, email security off)
- A warranty-eligibility or coverage-at-risk notice needs working
- A tech asks what a Cork posture signal means or whether it's an incident

## Steps

1. Read the signal as posture, not incident: which specific control slipped, on which asset/tenant, since when, and what the warranty requires for that control. Copy Cork's exact control/requirement wording. Route to the client per the desk's routing rules; low confidence → flag for a human.
2. Confirm the gap is real, not a reporting artifact: verify the control's actual state through the owning product's runbook (EDR gap → the EDR vendor runbook / edr-detection-runbook coverage check; MFA → the identity/MFA runbook; backup → the backup vendor runbook; email → the email-security runbook). A stale or mis-reported signal is a Cork data-quality issue, not a remediation — say which it is.
3. Decide the lane:
   - Pure compliance gap (control genuinely off/unhealthy, no threat evidence) → remediation with a warranty clock: restore the control, document the fix and the window it was out, so coverage isn't jeopardized.
   - Gap that also evidences active compromise (EDR was disabled by an attacker, backups deleted, MFA removed maliciously) → this is a security incident first — go to security-alert-response immediately; the warranty gap is secondary.
4. Prioritize by coverage risk and threat: a lapsed control that voids warranty on a high-value client, or one that doubles as an attack indicator, outranks a minor hygiene drift. State the business stakes plainly.
5. Drive remediation through the owning product (technician actions in that product's console) and confirm Cork re-reads the control as compliant afterward — the loop isn't closed until the posture signal clears.
6. Document the slipped control, real-vs-artifact verdict, the lane decision, remediation and out-of-compliance window, and the re-compliance confirmation. Client-facing wording per defensive-writing-standard — frame it as protecting their coverage, not blame.

## Guardrails

- A Cork signal is a warranty/posture gap by default, not an incident — don't over-escalate hygiene into emergency; but the moment a slipped control looks attacker-caused, flip to security-alert-response first.
- Verify the gap is real through the owning product before remediating — a mis-reported control is a data-quality fix, not a scramble.
- The out-of-compliance window matters for coverage — record when the control slipped and when it was restored; don't obscure a gap.
- The agent has no Cork console or the underlying product consoles — control remediation is a technician action in the owning product that the agent directs and records; the agent does not change warranty settings.
- Don't interpret warranty terms from memory — cite the client's specific warranty/documentation and mark anything unconfirmed.
- Degradation: without documentation (search_itglue), the client's required-control set and warranty terms may be unknown — say so before declaring a gap.
