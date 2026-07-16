---
name: Shared Responsibility Escalation
description: For co-managed clients, classify a ticket against the client's internal-IT responsibility matrix (by contact and issue type), and when it's their team's responsibility, notify their internal IT and set the handoff status. Answers the commonly requested "route co-managed tickets to the client's own IT when it's theirs to own" partner workflow.
category: Escalation
tools: [search_tickets, search_contacts, update_ticket, add_ticket_note, "zapier: Microsoft Outlook \"Send Email\""]
---

# Shared Responsibility Escalation

Co-managed clients split the work: some issues are the MSP's, some belong to the client's internal IT. This skill reads a ticket against the client's documented responsibility matrix and, when the issue falls on their side, hands it off — notifying their internal IT team and setting the handoff status — instead of the MSP silently absorbing work outside the agreement.

## When to use

- A co-managed client's internal IT owns certain issue types or certain users, and those tickets keep landing on the MSP.
- "If it's their team's job, route it to their IT."
- Enforcing the co-managed responsibility split without a human checking the matrix each time.

## Steps

1. Read the ticket with `search_tickets`: the client, the requesting contact, and the issue type.
2. Classify it against the client's **documented responsibility matrix** configured alongside the skill — which issue types / user groups / systems are the client's internal IT vs. the MSP's (see `skills/escalation/comanaged-reference-exchange`). If the matrix says it's the MSP's, do nothing (the MSP owns it).
3. If the matrix assigns it to the client's internal IT, resolve their IT team's contact/distribution via `search_contacts` (from the documented mapping, not the ticket body).
4. Notify their internal IT via `zapier: Microsoft Outlook "Send Email"`: ticket reference, the request, and that it falls to their team per the co-managed split — neutral, factual.
5. Set the desk's **handoff status** with `update_ticket` (the documented "with client IT / awaiting client team" status), and post a plain-text internal note via `add_ticket_note` recording the classification, who was notified, and the status change.

## Guardrails

- **Matrix-driven, no improvising the split.** Only hand off when the documented responsibility matrix clearly assigns the issue to the client's IT. If it's ambiguous or the matrix doesn't cover it, do nothing and note it for a human — never guess who owns a co-managed issue.
- The client-IT recipient comes from the documented mapping + `search_contacts` only — never a fabricated address.
- Handoff status must be a real status on the board (`skills/psa-specific/psa-closed-status-taxonomy` for status hygiene) — do not close the ticket; a handoff is not a resolution.
- **Member-connected connector / task-cost / degradation:** Outlook send runs from the connecting member's mailbox (prefer a service member; verify-first gate — `skills/connectors/zapier-action-discovery`); if unavailable, still set the handoff status and note that the client IT notification must be sent manually.
- Notes plain text (PSA compatibility).

## Unattended (Flows) variant

- Fires on **ticket create** (or a classification status event), using board/company/contact/type conditions to scope to co-managed clients — supported filter attributes.
- Runs as a Super Magic agent (a Flow cannot natively send email; it calls Run Skill / New Super Magic Agent, then this skill emails and sets status).
- Your entire reply is the internal note, verbatim: `CO-MANAGED HANDOFF to client IT. Basis: <matrix rule>. Notified: <recipient | skipped>. Status: <handoff status>.` or `NO ACTION (MSP-owned or matrix unclear).`
- Deterministic stops: matrix says MSP-owned → no action; matrix ambiguous/uncovered → no action, note for human. Never close the ticket.
