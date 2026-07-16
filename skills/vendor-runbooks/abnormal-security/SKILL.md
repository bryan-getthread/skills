---
name: Abnormal Security
description: An Abnormal email case landed — read its account-takeover and BEC/behavioral signals, treat an ATO case as an identity incident (not just an email one), and follow through on what auto-remediation didn't cover.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# Abnormal Security

Vendor specialization of security-alert-response for Abnormal, the behavioral email-security platform for Microsoft 365 and Google Workspace. Abnormal's strength is behavioral modeling — it flags business email compromise, vendor fraud, and account takeover by deviation from a learned baseline rather than by signature. The key triage insight: an Abnormal *account-takeover* case is an identity incident that happens to surface in email, and must be worked as compromised-account-containment, not filed as a phishing ticket. The generic runbooks own the canon; this skill maps Abnormal's case types onto them. Verify case-type names and remediation labels against Abnormal's current documentation.

## When to use

- An Abnormal case arrives: BEC / vendor fraud / invoice fraud, a phishing/malware verdict, or — most importantly — an Account Takeover case
- A tech asks how to read Abnormal's behavioral signals or whether a case is an identity compromise
- An Abnormal auto-remediation moved messages and the desk must validate and follow through

## Steps

1. Classify the case type first — it decides the runbook:
   - Account Takeover → this is an identity incident. Go straight to compromised-account-containment; the suspicious sign-in / mail-rule / send behavior Abnormal flagged is the symptom.
   - BEC / vendor fraud / invoice fraud → vendor-fraud-bec-alert: verify any payment/banking-change request through an independent, known channel — never the email thread. The absence of malware/links is the point; these attacks are pure social engineering.
   - Phishing / malware → phishing-triage / quarantine-release-request.
2. Parse the anatomy: affected identity (UPN), the behavioral signals Abnormal cited (unusual geo/device, atypical recipients, mailbox-rule creation, tone/urgency), the auto-remediation state (auto-remediated to junk/quarantine vs detected-only), and timestamps. Copy Abnormal's exact case language. Route per security-alert-response by tenant; low confidence → flag for a human.
3. For ATO, do not stop at the email symptom: run the full compromised-account-containment sweep (password, MFA methods, sessions, app passwords, delegated/OAuth access, mail rules) — Abnormal's message remediation does not touch the attacker's persistence.
4. Verify auto-remediation by effect: messages Abnormal moved should be confirmed moved; a claimed remediation that didn't apply is false comfort. Detected-only → treat as live.
5. Scope: search prior context (search_tickets, same client + sender/domain, 90 days) — vendor-fraud actors reuse infrastructure across a client's contacts.
6. Document case type, behavioral signals weighed, verification outcome, containment sweep results, and classify per soc-classification-tree. Client-facing wording per defensive-writing-standard.

## Guardrails

- An Account Takeover case is an identity incident — never close it as "email quarantined"; the sweep for persistence is the actual work.
- BEC/vendor-fraud verification goes through a known, independent channel (a phone number on file) — never reply-to the suspect thread; attackers control it.
- Auto-remediation is a claim until verified by effect; message movement does not equal identity containment.
- Never downgrade a behavioral verdict because "there's no malware" — the absence of payload is exactly how BEC works.
- Exclusions / VIP-impersonation exceptions are security decisions: narrowest scope, named approver, review date.
- The agent has no Abnormal or tenant-admin console access — remediation, un-remediation, and account containment are technician steps the agent directs and records.
- Degradation: without documentation access, the client's known-vendor and payment-approval process may be unknown — lean harder on independent verification and say so.
