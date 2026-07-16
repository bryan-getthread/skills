---
name: Password Expiry Change Guide
description: Draft reply-ready instructions for an end user to change a password that's about to expire (or just did) before it locks them out — "send the user steps to change their expiring password."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Password Expiry Change Guide

Produces a client-ready instruction block for a user whose password is expiring or has expired, walking them through changing it themselves the way the client's environment expects — before a lockout turns a two-minute self-service into a help-desk call.

## When to use

- "User got the 'your password expires in N days' warning — send them how to change it."
- "User's password expired and they can't sign in — send the change steps."
- Proactive nudge when an expiry report shows a user about to lapse.

## Steps

1. **Identify the client's change path first.** Check client docs (search_itglue / search_hudu / search_knowledge_base) and prior tickets (search_tickets): where passwords are changed for this shop — the Windows Ctrl+Alt+Del "Change a password" screen (domain-joined), the Microsoft/Entra "change password" page, a self-service portal, or the sign-in "your password has expired, create a new one" flow. Note the client's actual complexity rules and password history rule if documented (length, can't-reuse-last-N). If the path is unknown, ask the technician ONE question — never guess.
2. **Check the already-expired branch.** If the user is already locked out and can't reach a change screen, self-service change may not be possible — this can need a tech reset (cross-ref sspr-password-reset-guide if the client has self-service reset, otherwise flag to the tech). Only send change steps when the user can still authenticate.
3. **Draft the instruction block** to end-user rules, one action per step with what-you'll-see cues:
   - The entry point named exactly ("press Ctrl, Alt and Delete together, then choose 'Change a password'").
   - Old password once, new password twice, with a plain-language version of the client's actual rules ("at least 12 characters, and you can't reuse a recent one").
   - A tip for a strong, memorable passphrase — without ever suggesting they write it in the email.
   - The confirmation cue ("you'll see 'Your password has been changed'") and the day-to-day consequence: they must update the new password everywhere it's saved — phone mail, saved wifi, any app that remembered the old one — or those will lock the account with repeated retries.
   - Off-ramp: "If you get locked out or a screen doesn't match these steps, stop and reply — don't keep guessing, repeated tries can lock the account."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Never include or ask for the actual password** in the email, and never propose a specific password for them.
- **The "update it everywhere" warning is mandatory** — the #1 post-change lockout cause is a phone or saved credential retrying the old password until the account locks.
- Path match matters: a domain Ctrl+Alt+Del walkthrough sent to a cloud-only user fails at step one.
- No admin steps (resetting from the admin console, unlocking an account, changing expiry policy) in the user block — those are tech actions.
- If the account is already locked, do not tell the user to keep trying; route to a reset.
- Localizable; version-cautious UI cues.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- end-user-guides/sspr-password-reset-guide — when the user is already locked out and the client has self-service reset.
- end-user-guides/password-manager-start-guide — the "so I stop forgetting" follow-on.
- communication/email-baseline-standard.
