---
name: Compromised Account Containment
description: An account needs containing NOW — run the rapid checklist (block sign-in, revoke sessions, reset password, sweep MFA and rules) and timestamp every step in the ticket note.
category: Security
tools: [search_contacts, add_ticket_note, update_ticket]
---

# Compromised Account Containment

The five-move containment checklist as a standalone rapid skill. No investigation, no report-writing — stop the access, record the clock. The full workup lives in the account-takeover-runbook; this is the part that cannot wait for it.

## When to use

- Any skill or tech concludes "this account is compromised or credibly might be"
- A phishing victim entered credentials moments ago
- Mid-incident, an additional account turns out to be affected

## Steps

1. Block sign-in: disable the account or block interactive sign-in in the identity console. Timestamp it in the ticket note the moment it's confirmed done.
2. Revoke all active sessions and refresh tokens — blocking sign-in does not kill existing sessions; this step does. Timestamp.
3. Reset the password. Deliver the new credential out-of-band through a verified channel (voice, to a number on file) — never through the affected mailbox or any channel the attacker may read. Timestamp.
4. MFA sweep: review registered MFA methods and devices; remove anything the user does not recognize; timestamp each removal.
5. Rules-and-consent sweep: check inbox rules, forwarding, delegates, and OAuth app consents for attacker persistence; remove what's malicious; timestamp.
6. Post the running containment note as each step lands — not one note at the end. Format each line as plain text: timestamp, action, who executed it, outcome. Then hand off to the account-takeover-runbook for investigation, blast radius, and notification.

## Guardrails

- Contain fast, investigate second — this checklist runs on credible suspicion, before evidence is complete. The cost of containing a false positive is one inconvenienced user; the cost of waiting is not.
- Timestamps are mandatory on every step — the containment timeline is what insurers, auditors, and the postmortem all ask for. Document the decision, not just the action: the opening line of the note records why containment was triggered.
- Order matters: sessions must be revoked in addition to the password reset, or the attacker keeps riding the old tokens.
- Never communicate through the affected account, and never deliver the new credential to contact details sourced from the suspect session or ticket — verified, on-file channels only.
- The agent directs, verifies completion, and records; the technician executes the console actions.
- Containment is not closure — the ticket stays open and moves to the account-takeover-runbook. Never mark contained as resolved.
