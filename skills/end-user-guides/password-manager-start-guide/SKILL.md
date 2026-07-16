---
name: Password Manager Start Guide
description: Draft reply-ready first-steps instructions for an end user starting in the client's password manager — first sign-in, browser extension, saving a first password — "send the user getting-started steps for the password manager."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Password Manager Start Guide

Produces a client-ready instruction block for a user's first session in the company password manager — written for the specific product the client licenses (Keeper, 1Password, Bitwarden, LastPass, or another), because the invite flow, vault language, and extension behavior all differ by product.

## When to use

- "Send <user> getting-started steps for the password manager."
- Password-manager rollout tickets: users received invites and need a gentle on-ramp.
- "User was invited weeks ago and never activated — send them the steps again."

## Steps

1. **Identify the product and the client's rollout shape first** (search_itglue / search_hudu / search_knowledge_base / search_tickets): which product, whether sign-in is SSO (work account) or a separate master password, whether an invite email should already be in their inbox, and which browser extension the client deploys or expects users to install. If the product is unknown, ask the technician ONE question — password-manager terminology does not transfer between products.
2. **Draft the first-session flow** to end-user rules, one action per step, what-you'll-see cues:
   - **Get in:** find the invite email (say what the sender name will be, per the product) or the documented sign-in URL from client docs — never an invented URL. Cue the SSO-vs-master-password branch that actually applies: if SSO, "you'll sign in with your normal work login"; if master password, the plain rules: it's the one password they must remember, it should be long and unlike any other, and — critically — "we cannot recover it for you if it's lost" ONLY if that matches the product/client configuration in docs.
   - **Install the extension:** name the exact extension and browser(s) the client supports; cue what it looks like once installed (the icon near the address bar) and the sign-into-the-extension step users always miss.
   - **The first save:** have them sign in to one everyday website and accept the extension's save prompt — cue what the prompt looks like and where the saved item lands ("your vault — think of it as your personal locked notebook").
   - **The first fill:** sign out and back in to that same site using the extension, so they experience the payoff in the first session.
   - Off-ramps: "If there's no invite email, or sign-in says your account doesn't exist, stop and reply — your account may not be created yet." / "If the extension asks for anything that doesn't match these steps, stop and reply."
3. Include the two habits, briefly: let the manager generate new passwords rather than typing their own, and never share passwords by email or chat — use the manager's own sharing feature if the client has enabled it (verify before mentioning it).
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- Product match is mandatory — vault/collection/record terminology is product-specific and wrong terms make the guide unusable.
- Master-password recoverability claims must match the client's actual configuration (some deployments have admin recovery, some don't) — verify in docs or omit the claim; a wrong "we can't recover it" or "we can reset it" both cause harm.
- Never ask the user to send any password — not even "to check it's saved correctly" — and say so in the draft.
- Keep the first session to first steps: sign in, extension, one save, one fill. Import-from-browser, sharing, and cleanup are follow-on guides, not day one.
- No admin steps (user provisioning, policy, folders/collections admin) in the user block.
- Localizable; degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled.

## Cross-references

- troubleshooting-playbooks/password-policy-generator — tech-side password policy work.
- end-user-guides/new-computer-first-day-guide — where the extension re-install fits on a replacement machine.
- communication/email-baseline-standard.
