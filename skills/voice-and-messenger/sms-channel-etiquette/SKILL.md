---
name: SMS Channel Etiquette
description: The rules for texting clients — opt-in before anything, quiet hours, extreme brevity, and the hard list of what never goes over SMS (credentials, security details). Use when drafting a client text, deciding whether SMS is the right channel, or writing the desk's texting policy.
category: Voice & Messenger
tools: [search_contacts, search_tickets, add_ticket_note, "zapier: Twilio"]
---

# SMS Channel Etiquette

SMS is the channel clients actually read — which is exactly why it's the easiest one to abuse. This skill drafts client texts and channel decisions against the four rules that keep texting useful and legal: consent, timing, brevity, and content limits.

## When to use

- "Should we text the client about this, or email?"
- "Draft the SMS telling <user> the tech's ETA." (execution runs through Zapier Twilio SMS Updates)
- "Write our client-texting policy."
- Reviewing a proposed flow that includes SMS steps.

## Steps

1. **Channel fit first.** SMS is right for: time-critical one-liners (ETA, "call us back", outage pulse when email is the thing that's down), to a person whose action is needed now. SMS is wrong for: anything needing a paper trail the client will re-read, multi-step instructions, attachments, or any conversation — those go to email or Messenger. If it needs more than ~2 messages, it isn't an SMS use case.
2. **Opt-in gate.** Verify recorded consent on the contact (`search_contacts` / tenant policy) before drafting for send. No recorded opt-in → the answer is "not by SMS", with the alternative named. Consent is per-purpose where the tenant tracks it: service updates ≠ marketing. Regulatory exposure (TCPA-class and local equivalents) sits behind this gate.
3. **Quiet hours.** Default window 08:00–20:00 recipient-local (their site record). Outside it, only a P1 the recipient is actively engaged in justifies a text; otherwise schedule for window-open.
4. **Brevity + identification.** One message, under ~300 characters, MSP identified by name, one fact + one action: "<MSP>: your technician is en route, ETA ~20 min. Ref <ticket>." No greetings, no signatures, no link shorteners (they read as phishing). Reply expectations explicit: either "reply here" (someone is watching) or "don't reply to this number — call <number>".
5. **The never-over-SMS list** — content that does not go in a text regardless of convenience, because SMS is unauthenticated, unencrypted, and previewed on lock screens:
   - Credentials, temporary passwords, password-reset links, MFA codes or seeds,
   - Security-incident specifics ("your account was breached via...") — say "please call us urgently about your account" instead,
   - Client-confidential details, financial account data, anything regulated (health, card data),
   - Requests for the client to send credentials back.
6. Number handling: send only to the mobile on the contact record — never to a number quoted inside a ticket thread (social-engineering vector; same rule as Zapier Twilio SMS Updates, which owns the actual send mechanics).
7. Log every sent text to the ticket as a plain-text note: text, recipient, time. For policy-authoring requests, assemble rules 1–6 into a desk one-pager with the tenant's specifics (window, consent tracking location, approved use cases).

## Guardrails

- Opt-in and quiet hours are hard gates, not judgment calls. When consent is uncertain, do not send — say what record is missing.
- Sending requires a member-connected Zapier + tenant Twilio; degrade by producing the exact text and recipient for the member to send from their own tooling, and say that's what's happening.
- Inbound texts from clients are still tickets: anything received by SMS gets captured to the thread, not handled off-record.
- Never use SMS to verify identity ("text me the code you received" patterns are exactly what attackers do); identity verification follows the desk's security ladder on a trusted channel.
- Localizable and literal: no idioms, no humor that misfires in 160 characters; draft in the client's language.
