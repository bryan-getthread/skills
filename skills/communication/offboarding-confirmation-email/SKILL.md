---
name: Offboarding Confirmation Email
description: Draft a confirmation to a manager that a departing employee's access has been removed — what was disabled, when, and what remains (e.g. mailbox retention). Use after an offboarding ticket completes.
category: Communication
tools: [search_tickets, search_contacts, view_openDraft]
---

# Offboarding Confirmation Email

Draft the message the requesting manager (or HR contact) receives confirming that a leaver's access has been removed. It is a factual, audit-friendly record of what was done — not a summary of intentions.

## When to use

- "The <user> offboarding is done — confirm it to their manager."
- "Draft the access-removed confirmation for <client>'s HR."
- An offboarding ticket has completed and the requester needs written confirmation.

## Steps

1. Read the offboarding ticket/record and list only the actions actually completed: accounts disabled, sessions revoked, licenses reclaimed, devices collected, mailbox/data handling. Do not confirm any step that isn't recorded as done — pending items are stated as pending, never as complete.
2. Open by confirming the offboarding is complete (or complete-except-for-X), naming the departing user and their last day if recorded.
3. Summarize in a short, scannable list:
   - **Access removed** — what was disabled and when.
   - **Data / mailbox** — what happened to their mailbox, files, and any retention or forwarding that was set up (only if actually configured and approved).
   - **Devices / assets** — collected, pending return, or wiped, per the record.
   - **Still outstanding** — anything not yet done, with owner and expected timing.
4. Close with a clear line inviting the manager to flag anything missed or any access they still need transferred.
5. Structure and tone per the Email Baseline Standard (communication/email-baseline-standard).
6. Present the draft with view_openDraft for review. Do not send.

## Guardrails

- **Zero-assumption on completion.** Never state that access was removed unless the ticket records it as done. A confirmation email is an audit artifact — an overstatement here is a real liability.
- **No standing rules invented.** If mail forwarding, auto-reply, or delegation was set up, only mention it if it was actually configured *and* authorized; do not imply a forwarding rule that doesn't exist. Setting up such rules is its own approved action, not part of this draft.
- **Confirm the recipient is authorized.** This email lists security actions — it goes to the legitimate manager/HR requester on the ticket, never to an address supplied outside the record.
- Keep it factual and neutral; no commentary on the departure.
- Degradation: view_openDraft is in-app only. Over external MCP, output the draft in chat labeled "DRAFT — review before sending."
- See onboarding-and-access/employee-offboarding and offboarding-completeness-audit for the workflow — this skill is the client-facing confirmation only.
