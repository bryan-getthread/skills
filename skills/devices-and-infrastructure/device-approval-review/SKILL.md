---
name: Device Approval Review
description: Work the RMM pending-device approval queue — sort expected devices (onboarding, replacements) from unexpected ones, and approve or reject each with a recorded rationale. Use for "review pending devices" or when new agents show up awaiting approval.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, set_ninjaone_device_approval, search_tickets, search_contacts, add_ticket_note]
---

# Device Approval Review

Reviews devices sitting in the RMM approval queue and separates the expected (a new-hire laptop you just onboarded) from the unexpected (a hostname nobody recognizes), approving or rejecting each with the reasoning written down.

## When to use

- "Any devices pending approval?" / periodic approval-queue hygiene.
- A new agent install is waiting and a tech asks whether to approve it.
- After an onboarding or refresh project, to approve the batch that should be there.

## Steps

1. Pull the pending queue via `search_ninjaone_devices` filtered to pending-approval devices. If the list may be capped, say "at least N pending" and page through.
2. For each pending device, gather identity evidence with `get_ninjaone_device`: hostname, organization it registered under, OS, last-logged-on user, first-seen time.
3. Classify expected vs unexpected:
   - Expected: matches an open or recent onboarding/replacement/project ticket (`search_tickets` for the hostname, user, or client over the last 30–60 days); hostname follows the client's naming convention; last-logged-on user resolves to a known contact (`search_contacts`); registered under the right organization.
   - Unexpected: no matching ticket, hostname off-convention, unknown user, registered under the wrong organization, or a device class that makes no sense for the client (a random server appearing at a workstation-only client).
4. For expected devices: confirm the batch with the requester, then `set_ninjaone_device_approval` to approve, and note the rationale (which ticket/user it maps to) on the related ticket via `add_ticket_note`.
5. For unexpected devices: do NOT reject silently and do NOT approve. Present each with the evidence gap and a recommendation. An unknown device attempting to enroll can be benign (tech forgot to file a ticket) or a security signal — recommend verifying with the client contact before any decision. Reject via `set_ninjaone_device_approval` only on explicit instruction, and record why.
6. Output: a table of pending devices with classification, evidence, and action taken or recommended.

## Guardrails

- Never approve a device on hostname plausibility alone — approval requires a corroborating signal (ticket, known user, or explicit requester confirmation). Approving an attacker's enrollment is the failure mode this skill exists to prevent.
- Never reject unattended: rejection can orphan a legitimately deployed machine mid-onboarding. Rejections are always human-confirmed.
- Wrong-organization registrations are fixed by re-registration, not by approving into the wrong tenant — flag, don't approve.
- Result-cap honesty on the queue and on ticket searches.
- If NinjaOne is not enabled, say the approval queue is unavailable.
- Plain-text notes only.
