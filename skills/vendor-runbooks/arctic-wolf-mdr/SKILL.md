---
name: Arctic Wolf MDR
description: An Arctic Wolf escalation or ticket arrived — respect what their SOC already triaged, pick up exactly where their investigation ended, and work the response-authority split between what Arctic Wolf does and what the MSP must do.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# Arctic Wolf MDR

Vendor specialization of security-alert-response for Arctic Wolf managed detection and response. The defining difference from raw EDR alerts: an Arctic Wolf escalation has already been triaged by their SOC (Concierge Security / Security Teams model) — a human or pipeline validated it before it reached the desk. Re-triaging from zero wastes the paid-for work; blindly trusting it skips the desk's own obligations. This skill is the handshake. Verify service-tier names and escalation formats against Arctic Wolf's current documentation and the client's service agreement.

## When to use

- An Arctic Wolf incident escalation, investigation notice, or scheduled report lands as a ticket
- A tech asks "Arctic Wolf says X — what do we actually have to do?"
- Response actions need coordinating between the AW SOC and the desk's own technicians

## Steps

1. Read the escalation as a completed triage, not a raw alert: extract what AW observed, the evidence they cite, their severity/priority, what they have already done (their SOC can perform agreed response actions such as containment where the client contracted for it), and — the critical field — what they are asking the MSP to do. That ask is the work item.
2. Route to the correct client per security-alert-response and link any prior AW escalations for the same client/identity/host (search_tickets, 90 days) — MDR escalations often continue earlier threads, and AW references their own case IDs; preserve them verbatim in the ticket for round-trips.
3. Establish the response-authority matrix before acting — from the client's onboarding record (search_itglue; cross-ref mdr-client-onboarding, which sets this up): which actions AW is pre-authorized to take autonomously (e.g. host containment, account disable), which require MSP approval, and which are MSP-only (password resets, MFA re-registration, firewall changes, user communication, physical steps). If the matrix isn't documented, that is itself a finding — flag it and default to treating every action as requiring explicit MSP decision.
4. Do the desk's share, not a duplicate investigation: verify AW's containment claims took effect (account actually disabled, host actually isolated — check by effect), execute the MSP-side actions from the matrix (compromised-account-containment for identity cases, ransomware-response if that's the verdict), and handle all client-facing communication per defensive-writing-standard — AW talks to the MSP, the MSP talks to the client.
5. Answer their questions fast: AW escalations often include "is this expected?" verification asks (new admin account, new remote tool, travel login). These are time-sensitive — verify with the client contact via a number on file, never via a possibly-compromised mailbox, and respond to AW through the agreed channel with the answer and evidence. An unanswered verification ask silently becomes either a missed incident or a false-positive suppression.
6. Document the division of labor explicitly: what AW found/did, what the desk verified/did, the AW case ID, and the final verdict; classify per soc-classification-tree. Disagreements with an AW verdict are escalated back to their SOC with evidence — not silently overridden.

## Guardrails

- Never re-open triage from zero on an AW escalation without new evidence — but never skip verifying containment claims by effect, either. Trust, then verify.
- The response-authority matrix is the contract: taking an action AW was authorized to take (duplicate) or skipping one assumed to be theirs (gap) are both failures — check the matrix every time.
- AW verification questions ("is this expected?") get evidence-based answers on a deadline, not guesses; when the client contact can't be reached, say so to AW rather than answering "probably fine."
- Client-facing communication is the MSP's job and follows defensive-writing-standard; do not forward AW's internal-audience wording verbatim to clients.
- The agent has no Arctic Wolf portal access: portal acknowledgments, case replies, and console actions are technician steps the agent directs and records.
- Degradation: if the onboarding/authority documentation is absent, treat all response actions as requiring explicit MSP approval and flag the documentation gap per mdr-client-onboarding.
