---
name: Account Takeover Runbook
description: An account is confirmed or strongly suspected compromised — run the full response: disable sign-in, revoke sessions, reset MFA, sweep inbox rules and consents, assess blast radius, and notify.
category: Security
tools: [search_tickets, search_contacts, search_itglue, add_ticket_note, update_ticket, view_openDraft]
---

# Account Takeover Runbook

The complete response to a compromised account, in strict order: containment before investigation, eradication of persistence, blast-radius assessment, notification, and a timestamped record of every step. The agent drives the checklist and documents; the technician executes the console actions.

## When to use

- Impossible-travel, inbox-rule, or phishing triage escalated to suspected account takeover
- A user reports "someone sent emails from my account" or credentials were confirmed used
- A tech says "this account is compromised — walk me through the response"

## Steps

1. Contain fast, investigate second — direct the technician through the compromised-account-containment checklist immediately: block/disable sign-in, revoke all active sessions and refresh tokens, reset the password. Deliver the new credential through a verified out-of-band channel — never through the compromised mailbox. Record each action with a timestamp as it completes.
2. MFA sweep: list the account's registered MFA methods and devices; remove any the user does not recognize (attackers register their own method for persistence); re-enroll the user's legitimate method.
3. Persistence sweep beyond MFA: enumerate all inbox rules and forwarding addresses (attacker rules commonly forward externally, or hide replies by moving invoice/password/security keywords to RSS Feeds or Deleted Items — often named ".", "..", or a single character); review mailbox delegates; review OAuth application consents granted to the account and revoke unrecognized ones.
4. Blast-radius assessment: review sent items for outbound phishing or payment-fraud attempts during the compromise window; check whether other accounts at the client show similar sign-in anomalies; identify files or data shared out from the account. Search_tickets for related reports. Every recipient of attacker-sent mail becomes a phishing-triage follow-up.
5. Notify: reach the user by a number on file (not via the affected mailbox); notify the client contact per their documented incident policy, using the defensive-writing-standard — this is a "compromised account," confirmed at account level; it is not a "breach" unless investigation confirms system-level impact, and never "hacked."
6. Recovery gate: re-enable sign-in only after password rotation, session revocation, MFA re-enrollment, and persistence sweep are all confirmed complete.
7. Close out the record: the ticket note carries the full timestamped action log, the evidence, the blast-radius findings, and the decision reasoning. Classify per soc-classification-tree; closure status stays with management. If client-facing impact occurred, queue the security-incident-postmortem skill.

## Guardrails

- Containment starts on credible suspicion — do not wait for complete evidence to disable sign-in.
- Never communicate with the user through the compromised account or any channel from the suspect session.
- Every action gets a timestamp in the note — insurers, auditors, and postmortems all depend on the timeline. Document the decision, not just the action.
- The agent has no direct control of the identity console: it directs, verifies, and records; the technician executes.
- Sent-items evidence of payment-fraud attempts → immediately branch to vendor-fraud-bec-alert for every targeted recipient.
- Defensive writing in every client-facing message; disclosure decisions belong to management and the client's incident policy.
