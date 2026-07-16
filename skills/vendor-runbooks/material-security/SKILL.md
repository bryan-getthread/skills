---
name: Material Security
description: A Material alert landed — read its M365/Google email-security posture and data-access findings, treat a sensitive-data exposure or account-takeover signal as its own incident class, and follow through on protective actions Material staged.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# Material Security

Vendor specialization of security-alert-response for Material, the email-security and data-access-protection platform for Microsoft 365 and Google Workspace. Material is distinctive in what it protects: beyond phishing, it guards the *data already sitting in mailboxes* — it can redact/lock sensitive historical email behind step-up auth so that a mailbox takeover doesn't hand the attacker years of archived secrets. Triage therefore spans two lanes: email-threat events and data-access/posture events. The generic runbooks own the canon; this skill maps Material's event types onto them. Verify feature and alert names against Material's current documentation.

## When to use

- A Material alert arrives: a phishing/malicious email verdict, an account-takeover signal, a sensitive-data access/exposure event, or a posture finding (risky config, exposed archive)
- A tech asks how to read a Material alert or whether protected content was reached
- A Material protective action (content lock, step-up challenge) fired and the desk must validate

## Steps

1. Classify the event lane first:
   - Email threat (phishing/malware) → phishing-triage / quarantine-release-request.
   - Account takeover signal → compromised-account-containment; work it as an identity incident.
   - Sensitive-data access / exposure → treat as data-loss/exposure: dlp-alert-triage for the data-classification and scope questions, plus compromised-account-containment if an account is implicated.
   - Posture/config finding → route to remediation planning, not incident response, unless it evidences active exploitation.
2. Parse the anatomy: affected identity/mailbox, tenant/client, the specific data or message involved, whether Material's protection engaged (content locked, step-up required, access blocked) vs detect-only, and timestamps. Copy Material's exact wording. Route per security-alert-response by tenant; low confidence → flag for a human.
3. On an account-takeover with data implications, answer the exposure question precisely: did the attacker reach protected content, or did Material's step-up/lock hold? Confirm by effect in the console — a protection that engaged is containment-of-data, not containment-of-account. Run the full identity sweep regardless.
4. Verify staged protective actions by effect, don't assume; detect-only findings are live.
5. Scope: search prior context (search_tickets, same client + identity/indicator, 90 days).
6. Document event lane, whether protected data was reached, protection-engaged vs detect-only, sweep results, and classify per soc-classification-tree. Client-facing wording per defensive-writing-standard — be precise about what was and wasn't exposed.

## Guardrails

- Distinguish account containment from data containment — Material can hold the data while the account is still compromised; both need closing.
- Never state "no data exposed" without confirming the protection actually engaged and held in the console — precision on exposure is what the client (and any breach-notification duty) depends on.
- An account-takeover signal is an identity incident: run the persistence sweep, don't stop at the email symptom.
- Data-classification and exposure-scope decisions follow dlp-alert-triage — do not eyeball sensitivity.
- Posture findings are remediation items, not incidents, unless there's evidence of active exploitation — don't inflate the tier.
- The agent has no Material or tenant-admin console access — content unlocks, protection changes, and account containment are technician steps the agent directs and records.
- Degradation: without documentation access, the client's data-sensitivity policy may be unknown — say so and avoid guessing classification.
