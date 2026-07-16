---
name: Calendar Share Guide
description: Draft reply-ready instructions for an end user to share their calendar or grant delegate access from their side — "tell the user how to share their calendar / give someone delegate access."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Calendar Share Guide

Produces a client-ready instruction block for a user who needs to share their Outlook calendar or grant someone delegate access — the user-side steps — branched for Outlook web and desktop, and clear about the difference between "share" (view) and "delegate" (act on your behalf).

## When to use

- "User wants their assistant to manage their calendar — send them how to add a delegate."
- "User needs to share their calendar with a colleague so they can see free/busy."
- "How do I let someone book meetings for me?"

## Steps

1. **Confirm the platform and what the user actually needs** (search_itglue / search_hudu / search_knowledge_base / search_tickets): Outlook/Exchange is the common case. The key distinction to nail down: **sharing** (letting someone *see* the calendar, at a chosen detail level) vs. **delegation** (letting someone *manage* it — create/respond to meetings on the user's behalf). Note any client policy on external calendar sharing (often restricted or blocked). If the platform or external policy is unknown, ask the technician ONE question.
2. **Pick the branch that matches the need** — don't send delegate steps when they only want free/busy visibility, and vice versa.
3. **Draft the instruction block** to end-user rules, one action per step with what-you'll-see cues:
   - **Explain the choice plainly first:** "Do they just need to *see* your calendar, or *manage* it for you? Sharing shows it; delegate lets them accept and send meetings as you." Let the user confirm if unsure.
   - **Outlook desktop — delegate:** File → Account Settings → Delegate Access → Add → pick the person → set their permission level → choose whether they get meeting requests. Cue each dialog.
   - **Outlook desktop — share (view only):** open Calendar → Share Calendar → add the person → choose the detail level (free/busy, or full details). Cue the permission dropdown.
   - **Outlook on the web:** Calendar → Share (top right) → type the person → choose the permission level → Share. Cue the invite the recipient receives.
   - The permission-level line in plain terms: free/busy only vs. full details vs. edit — "share the least they need."
   - The external note: if the recipient is outside the company and the client allows it, expect a limited (often free/busy only) view; if it's blocked, the draft says so and routes to the desk.
   - Off-ramp: "If the person you're adding doesn't appear, or the sharing option is missing, stop and reply — that's usually a policy or account setting we handle."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Share-vs-delegate clarity is the skill** — granting full delegate access when someone only needed free/busy over-shares; the plain-language distinction appears in every draft.
- "Share the least they need" is the default posture — never default a user to full-details or edit access.
- External-sharing policy must be verified before drafting that branch; describing external sharing that the tenant blocks confuses the user.
- No admin steps (mailbox permissions via the admin console, org-wide sharing policy, PowerShell grants) in the user block — those are tech actions.
- Note that granting calendar access is a permission change the *user* performs on their own calendar; the agent does not grant permissions on the user's behalf.
- Localizable; version-cautious menu cues (Outlook classic vs. new differ).
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- end-user-guides/shared-mailbox-access-guide — the mailbox counterpart when the request is really about a shared mailbox.
- communication/email-baseline-standard.
