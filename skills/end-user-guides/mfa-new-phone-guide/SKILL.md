---
name: MFA New Phone Guide
description: Draft reply-ready instructions for an end user who got a new phone and needs to move or re-enroll their MFA safely — "user has a new phone and can't approve sign-ins."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# MFA New Phone Guide

Produces a client-ready instruction block for the single most common MFA lockout: a new phone. The steps branch hard on one question — does the user still have the old phone — because the safe path is completely different in each case.

## When to use

- "User got a new phone and MFA prompts are going nowhere."
- "Send <user> steps to move their authenticator to the new phone."
- Proactive: a ticket mentions an upcoming phone upgrade.

## Steps

1. **Verify the client's MFA product** (search_itglue / search_hudu / search_knowledge_base / search_tickets) — Microsoft Authenticator, Duo, Okta Verify, etc. If unknown, ask the technician ONE question before drafting anything.
2. **Determine which branch applies.** Check the ticket: does the user still have the old, working phone? If the ticket doesn't say, the draft asks that as its opening question and includes ONLY the still-have-it branch, with a clear "if the old phone is gone, stop here and reply — we'll reset it from our side" off-ramp. Never send both full branches interleaved; users execute half of each.
3. **Branch A — old phone still works.** Draft the transfer/re-enroll flow for the actual product:
   - Keep the old phone until the very last step — make this a bolded first line ("Don't wipe or trade in your old phone until we confirm the new one works").
   - One action per step with what-you'll-see cues: install the named app on the new phone, sign in to the security/enrollment page from a computer, remove-or-add the sign-in method, scan the new QR code, approve a test prompt.
   - For products with cloud backup/restore of the authenticator, mention it only if the client's docs confirm it's permitted — some clients disable it.
   - Off-ramp on every risky step: "If you're asked for a code from the OLD phone and it's already wiped, stop and reply."
4. **Branch B — old phone is gone/wiped.** The user cannot self-serve; re-enrollment requires an identity-verified MFA reset on the admin side. The user-facing draft says only: what will happen ("we'll clear the old registration after verifying it's really you"), how identity will be verified per the client's documented procedure, and what they'll do afterwards (fresh enrollment — link the flow from end-user-guides/mfa-setup-guide). The admin reset steps themselves never appear in the draft.
5. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Identity verification is non-negotiable in Branch B.** A "new phone, reset my MFA" request is a textbook account-takeover pretext — the draft must never promise a reset before the client's verification procedure runs, and the skill must remind the tech of that when producing Branch B. See security/account-takeover-runbook.
- Product match verified before drafting; no generic steps.
- Never instruct the user to disable MFA "temporarily," and never include admin-portal steps.
- No codes, QR images, or recovery keys transmitted over email in either direction.
- Localizable; draft in the thread's language. Version-cautious UI cues.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled.

## Cross-references

- end-user-guides/mfa-setup-guide — the fresh-enrollment flow Branch B hands off to.
- security/account-takeover-runbook — if anything about the request smells like a pretext.
- communication/email-baseline-standard.
