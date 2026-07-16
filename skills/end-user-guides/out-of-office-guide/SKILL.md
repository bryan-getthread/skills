---
name: Out of Office Guide
description: Draft reply-ready instructions for an end user to set their own out-of-office reply correctly — dates, internal vs. external messages — "send the user steps to set their OOO."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Out of Office Guide

Produces a client-ready instruction block for setting automatic replies properly — with the two traps handled: the internal/external split (most users don't know there are two messages) and the forgot-to-set-dates auto-reply that runs forever.

## When to use

- "Send <user> steps to set their out-of-office."
- Pre-vacation tickets and offboarding-adjacent leave tickets where the user sets it themselves.
- "User's OOO is still on / never turned on — send the how-to."

## Steps

1. **Verify the environment first** (search_tickets / search_itglue / search_hudu / search_knowledge_base): which Outlook the user reaches most easily (web works for everyone and survives version drift — prefer it as the primary path), and whether the client restricts external automatic replies (some do — if docs show external replies are blocked, the draft must say so instead of promising strangers will get the message). If the mailbox in question is a shared mailbox, stop — that's set on the admin side; tell the tech and don't send user steps.
2. **Draft the web-first flow** to end-user rules, one action per step with what-you'll-see cues:
   - Sign in to webmail; the settings gear; search or look for "Automatic replies" — cue: "a panel with an on/off switch at the top."
   - Turn it on, then **tick the send-only-during-a-time-period option and set both dates** — with the why: "this makes it switch itself on and off; without dates you'll be that person whose 'back on Monday' reply is still going out in August." Suggest starting it the evening before leave and ending it the morning of return.
   - The two message boxes, explained plainly: the first goes to coworkers, the second to everyone outside the company — "they can be the same text, but the outside one is the one clients see, so keep it professional and light on detail."
   - What a good message contains (offer a fill-in skeleton with bracketed placeholders): dates back, who to contact meanwhile (name/address the user supplies — never invented), and no more. Advise against oversharing travel details in the external message.
   - The optional calendar-tidy extras (decline/clear meetings during the period) only as a one-line "you may also see options to block your calendar — nice to have, optional."
   - Off-ramp: "If your settings don't show an Automatic replies option, stop and reply — your mailbox may be set up differently and we'll set it for you."
3. Include the verification step: "Send yourself a test from a personal address after it's on — you should get the external reply back within a minute or two" (only where external replies are permitted).
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- The set-both-dates instruction is the core of this skill — never draft an OOO guide without it and its plain-language why.
- Internal/external distinction appears in every draft; if the client blocks external auto-replies, say so honestly rather than letting the user believe clients are being answered.
- Never invent the covering colleague — the placeholder stays bracketed per the Email Baseline Standard's placeholder discipline, flagged "NEEDS: covering contact" if unknown.
- Shared-mailbox OOO requests never get user steps — route to the tech/admin side.
- The external message security note (don't advertise an empty house or exact travel plans) stays in.
- No admin steps (mailbox rules via admin center, transport rules) in the user block.
- Localizable; degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled.

## Cross-references

- end-user-guides/shared-mailbox-access-guide — the shared-mailbox boundary case.
- communication/holiday-coverage-notice — the desk-side coverage announcement counterpart.
- communication/email-baseline-standard.
