---
name: Password & MFA Recovery
description: Reset a password or recover MFA safely, with identity verification and a secure credential handoff.
category: Onboarding & Access
tools: [search_contacts, search_knowledge_base, add_ticket_note]
---

# Password & MFA Recovery

**When to use:** A user is locked out, needs a password reset, or has a new/lost authenticator device.

**What you get:** A completed reset with identity verified, MFA re-enrolled, and the temporary credential delivered securely, noted on the ticket without the secret.

## Steps

1. Verify the requester's identity per policy. Do not reset on an unverified request.
2. Reset the password and require a change at next sign-in, or walk MFA re-enrollment for a new device.
3. Confirm or enforce MFA. If compromise is suspected, revoke active sessions.
4. Deliver the temporary credential through a secure channel, never in the ticket body or chat.
5. Note the reset on the ticket without including the credential.

## Guardrails

- Never paste a password or one-time code into the ticket or chat.
- Escalate to compromise handling if there are signs of account takeover.

## Consolidates

Secure password reset, MFA enrollment, and MFA-reset/account-lockout skills.
