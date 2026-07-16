---
name: Update My Info Guide
description: Draft reply-ready instructions for an end user to update their own name, phone, or job title where self-service exists — "tell the user how to update their profile details."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Update My Info Guide

Produces a client-ready instruction block for a user who wants to update their own directory details — mobile number, job title, display name — pointing them to whatever self-service exists and honestly flagging the parts that require the desk or HR.

## When to use

- "User wants to update their mobile number / job title — send them how."
- "User's name changed (marriage, preferred name) — send the self-service steps and what we handle."
- "User's title in the directory is wrong — how do they fix it?"

## Steps

1. **Establish what's self-service vs. desk/HR-managed for this client first** (search_itglue / search_hudu / search_knowledge_base / search_tickets). This is the crux: in many Microsoft 365 tenants users can edit some fields in their own profile (often mobile phone) but display name, job title, and department are frequently **locked and synced from HR/on-prem AD**, so a user "updating" them either can't or gets overwritten at next sync. Note which fields this client lets users edit and where (the M365 profile / "My Account" / a self-service portal). If unknown, ask the technician ONE question.
2. **Sort the request into self-service vs. handled-for-you** and draft accordingly — don't send a self-service walkthrough for a field the user can't actually change.
3. **Draft the instruction block** to end-user rules, one action per step with what-you'll-see cues:
   - **Self-service fields:** the path (e.g. myaccount.microsoft.com or the org's portal → your profile → edit the allowed field), cue the Edit/Save controls, and note it can take a little while to appear everywhere (directory sync).
   - **Fields the desk/HR owns:** say plainly "some details like your display name and job title are set centrally so they stay consistent everywhere — reply here with the change you need and we'll take care of it (a name change may also need HR)." Never imply the user can self-edit a locked field.
   - The sync-lag reassurance: "after a change, it can take some time to show up in Teams, the address book, and email — that's normal."
   - Name-change note: a legal name change usually touches HR and sometimes the email address itself — that's a coordinated change, so reply and the desk will guide it.
   - Off-ramp: "If the field is greyed out or won't save, stop and reply — that usually means it's managed for you, not that you did anything wrong."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Self-service vs. managed verification is the skill.** Telling a user to change a title field that's HR-synced produces frustration and a "why didn't it stick" ticket.
- No admin steps (editing AD/Entra attributes, changing the primary email/UPN, HR system edits) in the user block — those are tech/HR actions; the draft routes to them.
- Never instruct the user to change their own sign-in name/email — UPN/primary-address changes are coordinated tech changes.
- The sync-lag note appears in every draft so users don't re-report a change that just hasn't propagated.
- Localizable; version-cautious portal cues.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- end-user-guides/email-signature-setup-guide — the follow-on when a title/phone change should also update their signature.
- communication/email-baseline-standard.
