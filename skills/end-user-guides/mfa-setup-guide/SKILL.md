---
name: MFA Setup Guide
description: Draft reply-ready instructions for an end user to enroll in multi-factor authentication with the client's actual MFA product — "send the user MFA setup steps."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# MFA Setup Guide

Produces a client-ready instruction block walking an end user through enrolling their authenticator app for the first time — written for the specific MFA product the client runs.

## When to use

- "Send <user> instructions to set up MFA."
- New-hire or security-rollout tickets where users must enroll an authenticator.
- "User keeps getting MFA prompts but never enrolled — send them setup steps."

## Steps

1. **Identify the client's MFA product first.** Check client docs (search_itglue / search_hudu / search_knowledge_base) and prior MFA tickets (search_tickets): Microsoft Authenticator via Entra, Duo Mobile, Okta Verify, Google Authenticator against a specific system, or a hardware token. Also note the enrollment entry point the client documents (for Microsoft it is typically the security-info page; for Duo an enrollment email/link). If the product is unknown, ask the technician ONE question — never send steps for a guessed product.
2. **Draft the instruction block** to end-user rules:
   - Two tracks kept clearly separated: (a) install the app on the phone, (b) enroll on the computer. One action per step.
   - What-you'll-see cues at every step ("A square QR code appears on your computer screen"; "Your phone will ask for camera permission — tap Allow").
   - Name the exact app to install ("Microsoft Authenticator — blue icon with a lock") so the user doesn't install a look-alike from the app store.
   - Include the confirmation step: the test prompt/code that proves enrollment worked, and what success looks like.
   - Off-ramp: "If the QR code won't scan, or any screen doesn't match these steps, stop and reply to this email."
   - Close with what changes for them day-to-day ("From now on you'll approve a prompt on your phone when signing in").
3. If the ticket shows the user has no smartphone or refuses to use a personal device, do not improvise an alternative — flag it to the tech (the client may offer SMS, hardware tokens, or a policy exception; that is a tech/admin decision).
4. **Assemble the email** per the Email Baseline Standard (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Product match is mandatory.** Duo steps sent to a Microsoft shop (or vice versa) fail at step one and burn a round-trip. If unverifiable, ask; don't guess.
- No admin-side steps in the user block (no conditional-access changes, no per-user MFA toggles) — if enrollment requires an admin action first, tell the tech, not the user.
- Never include or ask the user to share codes, QR screenshots, or backup keys in email.
- Version caution: app-store screens and portal layouts change; keep cues generic enough to survive minor UI updates and avoid pixel-precise claims ("look for a button named roughly 'Add sign-in method'").
- Localizable phrasing; draft in the thread's language.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only exist when enabled; fall back to KB and ticket history.

## Cross-references

- communication/email-baseline-standard — email structure and tone.
- end-user-guides/mfa-new-phone-guide — when the user is moving to a new phone rather than enrolling fresh.
- troubleshooting-playbooks/m365-signin-issues — tech-facing diagnostics when enrollment fails.
