---
name: Secure My Account Guide
description: Draft reply-ready first-response steps for an end user who thinks their account was hacked — change password, sign out everywhere, report — while routing the real incident to the tech — "the user thinks they were hacked, tell them what to do right now."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Secure My Account Guide

Produces a calm, client-ready first-response instruction block for a user who fears their account is compromised — the immediate self-protective steps they can safely take — while the skill simultaneously routes the real incident to the technician, because a suspected compromise is an incident, not a self-service ticket.

## When to use

- "User thinks they were hacked / clicked something / see logins that aren't them — tell them what to do now."
- "User got a 'your password was changed' or MFA prompt they didn't request."
- The user-facing half of an account-compromise report (the incident half goes to security).

## Steps

1. **Route the incident to the tech first — this is not optional.** A suspected compromise needs technician/security action (session revocation, sign-in log review, containment) that is beyond the user and beyond this guide. Flag it immediately per the client's incident process (cross-ref security/security-alert-response or the desk's escalation path). This skill produces the *user-facing* first-response reply that runs in parallel — it does not replace the incident.
2. **Verify the client's actual self-service surfaces before drafting** (search_itglue / search_hudu / search_knowledge_base / search_tickets): whether the user can change their own password and where, whether "sign out everywhere" is user-available (M365 users can revoke sessions from myaccount / mysignins), and the client's report path. Don't send a step the user can't perform in this environment.
3. **Draft the instruction block** to end-user rules, calm and numbered, one action per step with what-you'll-see cues:
   - Open with steadying reassurance: "You did the right thing telling us — let's lock things down together. We're already on it from our side too."
   - **Change the password now** (only if the user can still sign in): the client's change path, and make it a brand-new password not reused from anywhere. If they're already locked out, say "reply immediately and we'll reset it for you" — don't have them keep trying.
   - **Sign out everywhere / revoke sessions** if user-available: the exact path (e.g. mysignins.microsoft.com → security info / sign out of all sessions), cue what they'll see. This kicks out anyone still logged in.
   - **Check MFA is still theirs:** look at their sign-in methods for anything they don't recognize (an unknown phone/authenticator) — but only *report* what they see, not delete admin-side entries.
   - **Report and preserve:** don't delete the suspicious email/message — the desk needs it; don't try to "investigate" or click anything further.
   - Honest off-ramp with urgency: "If you entered your password on a suspicious site, or any money/banking or client data may be involved, call the desk right now instead of only emailing — minutes matter."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) — including its defensive, non-alarming tone — and present via view_openDraft. Do not send.

## Guardrails

- **Incident routing is mandatory and comes first.** This guide never stands alone as "the response" to a compromise — the tech/security flow runs in parallel; the draft is only the user's immediate self-protection.
- Only include self-service steps the user can actually perform in this tenant — verify; a "sign out everywhere" instruction that isn't user-available erodes trust mid-crisis.
- No admin/containment steps in the user block (revoking tokens org-wide, disabling the account, resetting MFA, reviewing audit logs, blocking sign-ins) — those are tech actions.
- Tone must reassure, not blame or alarm ("you were hacked" language is banned per the email baseline).
- The "call now if money/credentials/client data involved" off-ramp stays in every draft.
- Never ask the user to send their password or MFA codes in the reply.
- Localizable; the urgency of the call-now off-ramp must survive translation.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- security/security-alert-response — the tech/incident flow this guide runs alongside.
- end-user-guides/report-phishing-guide — when the trigger was a phishing email specifically.
- end-user-guides/password-expiry-change-guide — the mechanics of the password change itself.
- communication/email-baseline-standard.
