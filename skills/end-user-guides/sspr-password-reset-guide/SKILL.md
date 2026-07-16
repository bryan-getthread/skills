---
name: SSPR Password Reset Guide
description: Draft reply-ready instructions an end user can follow to reset their own password via self-service password reset — "send the user steps to reset their password themselves."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# SSPR Password Reset Guide

Produces a client-ready instruction block the technician sends to the end user so they can reset their own password through the client's self-service password reset (SSPR) — matched to the actual product the client runs, not a generic walkthrough.

## When to use

- "Send <user> instructions to reset their own password."
- "User forgot their password — can they self-serve?"
- A password-reset ticket where the client has SSPR enabled and the user just needs the steps.

## Steps

1. **Verify the client's SSPR stack first.** Search client documentation (search_itglue / search_hudu / search_knowledge_base) and prior tickets (search_tickets) for what this client uses: Microsoft Entra SSPR, Duo, ADSelfService Plus, Okta, or something else — and whether SSPR is actually enabled and whether this user is registered for it. If none of that is findable, ask the technician ONE question ("Which self-service reset does this client use, and is <user> registered for it?") — never send generic steps.
2. **Confirm the entry point.** Use the exact reset URL or "Forgot my password" path documented for that product for this client. Do not invent URLs; if the documented entry point is missing, flag it to the tech instead of guessing.
3. **Draft the instruction block** to end-user rules:
   - Plain language, no jargon (say "the sign-in page," not "the IdP portal").
   - Numbered steps, one action per step, each with a what-you'll-see cue ("You'll see a screen asking how you want to verify — text message or authenticator app").
   - Include the verification-method branch the user will actually hit (phone code, authenticator approval, security questions) based on what the product supports for this client.
   - Password requirements in plain terms only if documented for this client (length, no reuse); never invent a policy.
   - End with an off-ramp: "If any screen looks different from this, or you get an error, stop here and reply to this email — don't keep trying."
   - Add the practical aftermath step: sign back in on their computer and phone with the new password, since saved passwords (wifi, mail app) will prompt again.
4. **Assemble the email** per the Email Baseline Standard (communication/email-baseline-standard): one context line, the instruction block, next steps. Present via view_openDraft for the technician to review — do not send.

## Guardrails

- **Never send steps for a product the client doesn't run.** A Microsoft SSPR walkthrough sent to a Duo shop erodes trust and generates a callback. Stack verification is mandatory, not best-effort.
- If the user is NOT registered for SSPR, do not send this guide — tell the tech the reset must be done on the admin side, and offer to note that on the ticket instead. No admin-side steps ever appear in the user-facing block.
- Never include a password, a temporary password, or instructions to email a password.
- Too many failed attempts can lock the account — the off-ramp exists so the user stops before that happens; keep it in every draft.
- Keep phrasing localizable: no idioms; if the thread is in another language, draft the block in that language.
- Degradation: view_openDraft is in-app only. Over external MCP, output the finished draft in chat labeled "DRAFT — review before sending." search_itglue/search_hudu exist only when the docs integration is enabled; fall back to search_knowledge_base and ticket history.

## Cross-references

- communication/email-baseline-standard — email structure and tone.
- troubleshooting-playbooks/m365-signin-issues — tech-facing diagnostics if the reset fails.
