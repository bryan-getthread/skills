---
name: Approval Request Email
description: Draft an email to an approver requesting authorization — password reset, software install, purchase, access change — with what/why/risk/expiry stated so they can decide in one read.
category: Communication
tools: [search_tickets, search_contacts, send_approval, view_openDraft]
---

# Approval Request Email

Drafts the message that gets a decision: a request to a manager, account contact, or designated approver, structured so approving (or declining) takes one read and one reply.

## When to use

- "Ask <user>'s manager to approve this password reset."
- "Draft the approval request for this software install / purchase / admin access."
- Any change on a ticket that policy says needs sign-off from someone at the client.

## Steps

1. Read the ticket to establish what is being requested, for whom, and why. Identify the correct approver — the client's designated approval contact or the requester's manager, found via search_contacts or the client record. Never treat the requester as their own approver.
2. Draft per the Email Baseline Standard skill (communication/email-baseline-standard), with four labeled elements the approver can scan:
   - **What:** the exact action ("reset the password for <user>'s account", "install <software> on <device>", "purchase <item> at <cost>").
   - **Why:** the business reason in one sentence, tied to the ticket.
   - **Risk / impact:** what this changes and any cost, downtime, or access implication — stated honestly, including "low risk" when true.
   - **Expiry:** how long the request stands ("if we don't hear back by <date/time>, we'll put this on hold and check in").
3. Make the response mechanics trivial: "Reply 'approved' to this email and we'll proceed."
4. If Thread's structured approval feature fits (a yes/no decision on a ticket), prefer send_approval so the decision is captured on the ticket; use the email draft when the approver is outside that workflow or the request needs the full what/why/risk/expiry context.
5. Present as an open draft via view_openDraft (or alongside the send_approval action for confirmation). Do not send or trigger the approval without the technician's go-ahead.

## Guardrails

- Never perform the requested action in anticipation of approval. Draft, send (with confirmation), wait.
- Identity discipline for security-sensitive approvals (password/MFA/access): the approver must be a contact on file, never an address supplied inside the ticket text itself.
- State cost figures only if they appear in the ticket or a quote; never estimate prices into an approval request.
- Expiry dates must be real commitments the desk will honor.
- Degradation: view_openDraft and structured approvals may be unavailable over external MCP — output the draft in chat labeled "DRAFT — review before sending" and note who it should go to.
