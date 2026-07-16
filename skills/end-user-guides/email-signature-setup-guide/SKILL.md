---
name: Email Signature Setup Guide
description: Draft reply-ready instructions for an end user to set up or update their email signature in Outlook (web and desktop branches) — "send the user how to set their signature."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Email Signature Setup Guide

Produces a client-ready instruction block for a user setting up or changing their email signature, branched for Outlook on the web and Outlook desktop, and honest about whether the client applies signatures centrally.

## When to use

- "New hire needs their signature set up — send them the steps."
- "User wants to update their title/phone in their signature."
- "User's signature isn't showing on replies — send the settings walkthrough."

## Steps

1. **Check how this client manages signatures first** (search_itglue / search_hudu / search_knowledge_base / search_tickets). This is the make-or-break question: if the client uses a **centralized signature tool** (e.g. Exclaimer, CodeTwo) or a server-side transport rule, the user must NOT create their own — the draft instead explains the signature is applied automatically, what to expect, and who to contact for a correction. Only send self-service steps if users manage their own. Also note whether the client provides a signature template/brand standard the user must paste in. If unknown, ask the technician ONE question.
2. **Confirm which Outlook the user is on** from the ticket (web vs. desktop, Windows vs. Mac). If unclear, draft both branches under clear headings so the user picks.
3. **Draft the instruction block** to end-user rules, one action per step with what-you'll-see cues:
   - **Outlook on the web:** Settings (gear icon, top right) → search or open "Signatures" under Mail → Compose and reply. Cue what the box looks like; note the two toggles for "use on new messages" and "use on replies."
   - **Outlook desktop:** File → Options → Mail → Signatures (or the new Outlook: Settings → Accounts → Signatures). Cue the New button and the same new-vs-reply assignment.
   - If there's a client template, tell them to paste it and only change the placeholder fields (name, title, direct line) — not the logo, colors, or layout.
   - Reassure that web and desktop signatures are stored separately, so setting one doesn't set the other.
   - The success test: send one email to themselves and check it looks right on both a computer and a phone.
   - Off-ramp: "If the logo doesn't appear, or the signature looks broken on your phone, stop and reply — paste-from-Word breakage is common and we'll fix the source."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Centralized-signature check is mandatory.** Telling a user to hand-build a signature when the client stamps one server-side creates duplicates on every email — the most visible desk mistake there is.
- Keep brand fidelity: users edit their own details only, never the client's template design.
- No admin steps (transport rules, the signature-tool admin console, disabling the server signature) in the user block.
- Warn against pasting formatted content from Word/websites, which carries hidden styling that breaks on mobile — paste as plain text or use the client's template.
- Localizable; version-cautious menu cues (Outlook's "new" vs. "classic" layouts differ).
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- end-user-guides/update-my-info-guide — when the driver is a name/title/phone change that should propagate elsewhere too.
- end-user-guides/outlook-profile-setup-guide — the broader Outlook setup this often rides along with.
- communication/email-baseline-standard.
