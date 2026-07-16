---
name: Mobile Mail Setup Guide
description: Draft reply-ready instructions for an end user to get work email on their phone — Outlook mobile app path first, matched to the client's mobile policy — "send the user steps for email on their phone."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Mobile Mail Setup Guide

Produces a client-ready instruction block for setting up work mail on a phone. Default path is the Outlook mobile app (it works the same on iPhone and Android and plays well with security policies); native mail apps only if the client's policy explicitly allows them.

## When to use

- "Send <user> instructions to get email on their phone."
- New hire or new phone tickets where mobile mail is part of setup.
- "User's phone stopped getting mail after the security rollout — send re-setup steps."

## Steps

1. **Verify the client's mobile policy first** (search_itglue / search_hudu / search_knowledge_base / search_tickets): Is there MDM/app protection (e.g., Intune) that requires an extra companion app or blocks native mail? Are personal devices allowed at all? Is this a corporate-managed phone (in which case mail may deploy automatically and no guide is needed)? If policy is unknown, ask the technician ONE question — sending native-app steps to a client that blocks native mail creates a "broken" experience the user blames on IT.
2. **Draft the Outlook-app path** (the default) to end-user rules:
   - One action per step: install "Microsoft Outlook" from the official app store (name the store per platform; describe the icon), open it, enter the full work email address, complete the same sign-in they use for webmail, expect the MFA prompt.
   - What-you'll-see cues throughout ("The first screen asks only for your email address — nothing else").
   - If the client uses app protection, warn about the expected extras in plain terms: "You may be asked to install a second app or set a PIN for the mail app — that's our security policy, it's expected, go ahead."
   - Honest wait note: recent mail appears in minutes; the full mailbox trickles in.
   - Off-ramps: "If you're asked for server names or ports, stop and reply." / "If your phone says the app is 'managed' and you're not comfortable, stop and reply — we'll explain what that means before you continue."
3. Include the native-mail branch ONLY if client docs confirm it's permitted, and even then as a secondary "if you strongly prefer the built-in mail app" section — never as the lead.
4. Personal-device consent: if this is a personal phone and the client requires enrollment/app protection, the draft must say plainly what the company can and cannot do on their phone per the client's documented policy — and if that policy text isn't documented, flag to the tech rather than improvising a privacy statement.
5. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- Policy verification is mandatory — the failure mode isn't wrong steps, it's steps that silently hit a policy wall and generate a frustrated callback.
- Never draft privacy/consent claims about MDM beyond what the client's documentation states.
- No admin-side steps (no Intune console, no conditional-access changes) in the user block.
- No server settings, no app passwords, no "less secure app" workarounds — ever.
- Localizable; version-cautious cues; iPhone/Android differences kept to the install step where possible (the Outlook app converges after that).
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled.

## Cross-references

- end-user-guides/outlook-profile-setup-guide — the desktop counterpart.
- troubleshooting-playbooks/mobile-device-mdm — tech-facing diagnostics when enrollment fails.
- communication/email-baseline-standard.
