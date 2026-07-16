---
name: Voicemail Setup Guide
description: Draft reply-ready instructions for an end user to set up or change their voicemail and greeting on the client's actual phone system — "send the user how to set up their voicemail."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Voicemail Setup Guide

Produces a client-ready instruction block for a user setting up voicemail for the first time or changing their greeting, written for the specific phone system the client runs — the steps differ completely between Teams Phone, a hosted PBX, and a desk-phone platform.

## When to use

- "New hire needs their voicemail set up — send them the steps."
- "User wants to change their voicemail greeting (out sick / going on leave)."
- "User isn't getting voicemails / hasn't set a PIN — send setup steps."

## Steps

1. **Identify the client's phone system first** (search_itglue / search_hudu / search_knowledge_base / search_tickets): Microsoft Teams Phone, a hosted VoIP platform (RingCentral, 8x8, Zoom Phone, etc.), or a desk-phone/PBX system. The setup surface differs — Teams voicemail lives in Teams settings and email; a hosted platform has an app and/or a dial-in feature code; a desk phone uses a physical Messages button and a mailbox PIN. Note the client's dial-in access number or feature code if documented. If the system is unknown, ask the technician ONE question — never guess a feature code or access number.
2. **Confirm what the user actually needs** from the ticket: first-time setup (PIN + name recording + greeting) vs. just changing the greeting. Draft only what's needed.
3. **Draft the instruction block** to end-user rules, one action per step with what-you'll-see cues, in the branch that matches the client's system:
   - **Teams Phone:** Teams → Settings → Calls → Voicemail → Record/Configure greeting; note voicemails also arrive in email with a transcript. Cue each screen.
   - **Hosted VoIP:** open the provider's app (name it) → the voicemail/greeting section, OR dial the documented feature code from their phone and follow the voice prompts. Cue the menu ("press 1 to record your greeting").
   - **Desk phone:** press the Messages/voicemail button, enter the temporary PIN, follow prompts to set a new PIN, record their name, then record the greeting. Cue each prompt.
   - A short sample greeting they can adapt (name, "I can't take your call," when they'll respond, alternative contact) — never a real name; use "your name."
   - The success test: call their own line and leave a test message, confirm they receive it.
   - Off-ramp: "If the feature code doesn't work or you don't have a temporary PIN, stop and reply — we'll get you the right access details."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Never invent a dial-in access number, feature code, or PIN** — these are environment-specific; docs or ask are the only sources.
- System match is mandatory; Teams voicemail steps sent to a desk-phone shop don't apply.
- Never include or ask the user to send their voicemail PIN in email.
- This is a user-facing setup guide, not telephony administration — no steps for call routing, hunt groups, number porting, or PBX config (those are admin/tech and, for call routing, outside the agent's control entirely).
- Localizable; version-cautious app and menu cues.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- end-user-guides/out-of-office-guide — the email counterpart when the user is setting an away greeting because they're out.
- communication/email-baseline-standard.
