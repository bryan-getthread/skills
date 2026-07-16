---
name: Recall or Replace Email Guide
description: Draft reply-ready instructions for an end user who wants to recall or replace an email they just sent — with honest limits on when recall actually works — "tell the user how to unsend that email."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Recall or Replace Email Guide

Produces a client-ready instruction block for a user who sent an email by mistake, walking them through the recall/replace path their mail system actually supports — and, crucially, setting honest expectations, because recall only works under narrow conditions and never for external recipients.

## When to use

- "User sent an email to the wrong person and wants to recall it."
- "User attached the wrong file — can they replace the message?"
- "How do I unsend an email?" tickets.

## Steps

1. **Establish the honest reality first, then the mechanism.** Check client docs and prior tickets (search_itglue / search_hudu / search_knowledge_base / search_tickets) for the mail platform. Recall behavior differs sharply: classic Outlook desktop "Recall This Message" only works when **both sender and recipient are inside the same organization on Exchange and the recipient hasn't opened it**; it does nothing for external recipients or most mobile/other clients. The web/new Outlook "Undo Send" is different — a few-second window right after sending, set in settings. If the client platform is unknown, ask the technician ONE question.
2. **Triage urgency by recipient.** If the email went **outside the organization** or contained sensitive data, recall will not help — the draft says so plainly and routes the user to notify the desk/tech immediately (a data-exposure question, not a self-service one). Do not imply an external email can be pulled back.
3. **Draft the instruction block** to end-user rules, one action per step with what-you'll-see cues:
   - Lead with the honest frame: recall only works inside the company and only if they haven't opened it yet — "it's worth trying, but don't count on it."
   - **Classic Outlook desktop branch:** open the sent message from Sent Items, Message tab → Actions → Recall This Message, choose delete-or-replace, cue the confirmation. Note they'll get a per-recipient success/failure report.
   - **Undo Send branch (web/new Outlook):** only the brief window right after sending — cue the "Undo" banner at the bottom — and how to lengthen it in settings for next time.
   - Set expectations honestly: the recipient may still see the original or a "sender tried to recall" notice; recall can backfire by drawing attention.
   - The better habit going forward: proofread and re-check the To line before sending; consider a longer Undo-Send window.
   - Off-ramp: "If this went to someone outside the company, or it had anything sensitive in it, don't rely on recall — reply here right now so we can advise."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Honesty about limits is the whole point.** Never promise an email can be unsent — overpromising here erodes trust the moment recall silently fails.
- The external-recipient / sensitive-data off-ramp appears in every draft; that scenario is an incident, not a settings tweak.
- Platform match matters: classic-desktop recall steps sent to a web-only user don't exist for them.
- No admin steps (message trace, admin recall via compliance tools, mail-flow investigation) in the user block — those are tech/admin actions.
- Localizable; version-cautious menu cues (Outlook "classic" vs. "new" differ).
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- security/phishing-triage — if the "wrong email" turns out to be a compromise or data-exposure incident.
- communication/email-baseline-standard.
