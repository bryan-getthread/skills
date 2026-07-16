---
name: Onboarding Welcome User Email
description: Draft a day-one welcome email to a brand-new end user — who we are, how to reach support, and what's already set up for them. Use when a new hire's account is provisioned and they need a friendly first touch.
category: Communication
tools: [search_tickets, search_contacts, view_openDraft]
---

# Onboarding Welcome User Email

Draft the first message a newly onboarded end user receives from the service desk: a warm introduction, the ways to reach support, and a plain summary of what has been set up for them. The goal is a confident, low-friction first day — not a wall of instructions.

## When to use

- "A new hire at <client> just got provisioned — send them a welcome."
- "Draft the day-one email for <user>."
- An onboarding ticket is complete and the end user should be introduced to support.

## Steps

1. Confirm from the onboarding ticket/record what was actually provisioned for this user (email, laptop, key apps, MFA) before writing. Only mention what is confirmed set up. Anything uncertain goes in `NEEDS:` at the top, not into the body as a guess.
2. Open with a brief, friendly welcome by first name — this is the first message of the relationship.
3. Cover, in short plain-language sections:
   - **Who we are** — one line: the IT support team for <client>.
   - **How to reach us** — the supported channels (email/portal/Messenger) that actually exist for this client. Do not list a channel the client doesn't have.
   - **What's set up for you** — a short list of what's ready, drawn from the ticket.
   - **First steps** — anything the user must do on day one (e.g. set up MFA), only if that step is real and pending.
4. Keep it scannable — a new hire is busy. One short intro paragraph plus a tight list.
5. Structure and tone per the Email Baseline Standard (communication/email-baseline-standard).
6. Present the draft with view_openDraft for review. Do not send.

## Guardrails

- **No credentials in email.** Never include passwords, temporary passwords, MFA codes, PINs, or reset links in the welcome message. Direct the user to the proper secure path for first sign-in. (Entering or sending credentials is out of scope entirely.)
- **Only what's confirmed.** Do not promise access or apps that aren't actually provisioned for this user.
- **Match the client's real channels.** Don't invent a portal or phone line the client doesn't use.
- Keep it welcoming and short — this sets the tone for every future interaction.
- Degradation: view_openDraft is in-app only. Over external MCP, output the draft in chat labeled "DRAFT — review before sending."
- See onboarding-and-access/new-hire-onboarding for the provisioning workflow — this skill is the client-facing welcome only.
