---
name: Approver Self-Skip
description: Before firing an approval, check whether the ticket submitter IS the designated approver for that client — if so, skip send_approval and advance the ticket with an audit note instead of making an approver approve their own request. Answers the commonly requested "don't ask the approver to approve themselves" workflow.
category: Automation & Flows
tools: [search_contacts, send_approval, update_ticket, add_ticket_note, list_ticket_statuses]
---

# Approver Self-Skip

A deterministic pre-check for any approval gate: when the person who submitted the ticket is the client's designated approver, sending them an approval request is pure friction. Detect the self-approval case from the contact record, skip the send, and advance the ticket with an audit note. Companion to `automation-and-flows/new-ticket-approval-gate` (the intake gate this most often wraps) and `automation-and-flows/change-approval-sender`.

## When to use

- A Flow reaches its send-approval step and the desk wants self-submitted requests from approvers to pass straight through.
- The client's office manager (the designated approver) files "please add a mailbox for <user>" and then has to approve their own ticket.
- Any approval-gated intake where approvers are also frequent requesters.

## Steps

1. Resolve the ticket's submitter contact and the client's designated approver(s) via `search_contacts`: contacts for this client carrying the desk's approver contact type/label.
2. Compare identities **from the contact records only**: the submitter is the approver iff their contact record matches an approver contact (same contact ID, or same verified email on the contact record). Name similarity, email display names, or "probably the same person" do NOT count.
3. **Match** → skip `send_approval`. Advance the ticket out of the approval-waiting status via `update_ticket` (use `list_ticket_statuses` to resolve the desk's configured post-approval status) and post a plain-text audit note via `add_ticket_note`: "Approval auto-skipped: submitter <contact> is the designated approver for <client> (contact-type match). No approval request sent."
4. **No match, or cannot resolve** → send the approval normally via `send_approval` to the designated approver. The skip is the exception; the send is the default.
5. Output/post: which branch was taken and why, in one line.

## Guardrails

- **Identity from the contact record only.** Email in the ticket body, display names, or inferred aliases never qualify. If the submitter arrived via an unverified channel (e.g. plain email with no matched contact), always send the approval.
- The skip is deterministic, not judgment-based: exact contact match → skip; anything else → send. There is no "close enough".
- If the client has multiple designated approvers, the submitter must match one of them exactly; if the desk configured a *specific* approver per request type, match that one.
- Always leave the audit note when skipping — a silently advanced ticket looks like a missed gate in QA.
- Never skip approvals the desk marked as always-required (e.g. spend above a threshold, offboarding) even for self-submitted tickets; that list lives in the desk's config for this skill.
- Degradation: if `search_contacts` cannot resolve the approver contact type for this client, do not guess — send the approval to nobody, post a note that the approver is unresolved, and stop (mirror new-ticket-approval-gate's note-and-stop posture).

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the internal note — plain text, no narration, no markdown.
- Exactly three outcomes: (a) exact match → skip, advance status, audit note; (b) no match → send_approval as configured, one-line note; (c) unresolved → no send, no status change, note "Approver unresolved for <client>; approval not sent; needs human."
- Never advance status without the audit note; never send AND skip.
