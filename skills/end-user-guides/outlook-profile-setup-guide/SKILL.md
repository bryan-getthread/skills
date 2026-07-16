---
name: Outlook Profile Setup Guide
description: Draft reply-ready instructions for an end user to add their work account to Outlook on Windows or Mac — "send the user steps to set up Outlook."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Outlook Profile Setup Guide

Produces a client-ready instruction block for adding the work email account to desktop Outlook — branched by platform (Windows classic Outlook, new Outlook for Windows, Outlook for Mac), with only the branch that matches the user's machine.

## When to use

- "Send <user> instructions to add their email to Outlook."
- New machine, new profile, or profile-rebuild tickets where the user will do the add themselves.
- "User's Outlook profile was removed — walk them through re-adding the account."

## Steps

1. **Verify the environment first.** From the ticket, prior tickets (search_tickets), and client docs (search_itglue / search_hudu / search_knowledge_base): (a) is the user on Windows or Mac, (b) which Outlook — classic Outlook, the new Outlook for Windows, or Outlook for Mac, and (c) is the mailbox Microsoft 365 (the normal case) or something else (hosted Exchange, IMAP) — the flow differs completely. If platform or mailbox type is unknown, ask the technician ONE question; do not send a two-platform mega-guide.
2. **Draft the single matching branch** to end-user rules:
   - One action per step with what-you'll-see cues ("A window titled 'Add account' opens with one empty box for your email address").
   - The user enters their full work email address; modern sign-in then takes over — tell them the sign-in page will look like the one they use for webmail, and that an MFA prompt on their phone is expected.
   - State the wait honestly: "Your mail and calendar will fill in over the next several minutes to a few hours depending on mailbox size — old items appear last."
   - Off-ramps: "If you're asked for a server name, port, or anything technical — stop and reply; you should never need those." and "If sign-in loops or errors, stop and reply with a photo of the screen."
3. Do not include admin-side material: no Autodiscover records, no registry keys, no license assignment. If docs suggest the mailbox/license isn't ready, flag to the tech before drafting.
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Steps-cue notes

- Classic Outlook (Windows): File → Add Account. New Outlook for Windows: gear/Settings → Accounts → Add account. Mac: Outlook menu → Settings → Accounts → +. Use these as orientation cues, phrased version-cautiously ("look for Add Account under the File menu — or under Settings if your Outlook looks newer").

## Guardrails

- Platform and Outlook-flavor match is mandatory — Mac steps to a Windows user is an instant failed reply. When the flavor is ambiguous, the draft's first line asks one identifying question ("Does your Outlook have a 'File' menu in the top-left? Reply yes or no") and defers the rest.
- Never include server settings, ports, or manual-configuration paths in a user draft — if manual config is genuinely needed, that's a tech-driven session, not an email.
- Never ask the user to send their password; the draft should explicitly say "we will never ask for your password by email."
- Localizable; version-cautious cues (Outlook UI changes frequently).
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled.

## Cross-references

- troubleshooting-playbooks/outlook-client-issues — tech-facing diagnostics when the add fails.
- end-user-guides/mobile-mail-setup-guide — the phone counterpart.
- communication/email-baseline-standard.
