---
name: Email Issues Intent Design
description: Build the email-problems intent — triage questions that separate mailbox, client-app, and mail-flow problems before a ticket is cut, so "email is broken" arrives classified. Use when asked to "build an email issues intent", "deflect email tickets", or when email ranks high in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets, search_knowledge_base]
---

# Email Issues Intent Design

Guide the admin through building an email intent whose value is classification: three or four questions that tell mailbox problems (account/quota/access), client problems (Outlook/app misbehaving), and mail-flow problems (messages not arriving/leaving, bounces) apart — because those route to different fixes and different urgency.

## When to use

- "Build an intent for email problems."
- "'Email not working' tickets take three replies just to find out what's wrong."
- Intent Mining flagged email as high volume but low current automatability — classification is the fix.

## Design

**Trigger phrases (adapt to real ticket language):** "email not working", "not receiving emails", "can't send email", "emails bouncing", "outlook not working", "outlook keeps asking for password", "mailbox full", "emails going to spam", "missing emails", "email stuck in outbox", "can't open my email".
Near-miss watch: "set up email on my phone" is the mobile-setup intent; "need access to the shared mailbox" is an access request; "suspicious email" should route to the client's phishing/security process, not this intent.

**Arguments to collect (the classifier):**
1. **Direction:** sending, receiving, or both? Specific senders/recipients or everyone?
2. **Where:** one device/app or everywhere (webmail too)? "Works in webmail, broken in Outlook" = client problem.
3. **Evidence:** exact bounce/error text if any (bounce codes are gold — capture verbatim).
4. **Scope:** just this user or colleagues too? (Tenant-wide flow issue escalates.)
5. When it last worked / what changed (password change, new phone, mailbox nearly full warning).

**Reply flow:**
1. Ask the classifier questions (adaptive — skip what the user already said).
2. **Client-app signature** (webmail works, app doesn't) → offer the one safe self-help rung: fully close and reopen the app, then restart the computer; if the app demands a password after a recent password change, sign in fresh once — never loop credential retries. If unresolved → ticket classified "email client".
3. **Mailbox signature** (quota warnings, can't sign in anywhere) → ticket classified "mailbox/account"; sign-in failures follow the verified identity path, no credential handling in chat.
4. **Mail-flow signature** (bounces, missing mail, spam-foldering, multiple users) → ticket classified "mail flow" with bounce text verbatim; multiple users affected flags possible outage and escalates.
5. Every ticket carries the classifier answers in a plain-text block.

**Handoff rule:** no mailbox-permission, DNS, transport-rule, or credential steps in chat. Suspicious-message reports divert to the security process immediately.

**Variation hooks (per client):** mail platform (M365/Google/hosted), webmail URL, phishing-report route, whether a status page exists to check first.

**Success metric:** classification completeness — percentage of email tickets arriving with direction/where/scope/error captured — plus the small deflection slice from the client-restart rung. Watch misclassification complaints from techs as the counter-metric.

## Steps

1. `list_intents` — check for an existing email intent; prefer updating.
2. `search_tickets` for recent email tickets; mine phrasing for triggers, and read resolutions to validate the three-way classification against this desk's reality.
3. `search_knowledge_base` for existing email troubleshooting articles; link where relevant.
4. Draft the full spec: triggers, classifier questions, the three signatures and their routing, variations. Test plan: 5 should-match, 3–5 should-not (mobile-setup, shared-mailbox-access, phishing near-misses).
5. Show the draft; on explicit confirmation call `create_intent` then the variation tools.
6. Report what was created, restate the test plan, recommend activation after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, output the complete written spec for an admin to apply.
- Suspicious/phishing mentions divert to the security process — never triage a potential compromise as a routine email issue.
- No credential handling, no DNS/transport advice, no mailbox-permission steps in replies.
- Multiple users affected escalates as a possible outage; never hold it in classification questions longer than necessary.
- Confirm before any write; never activate on the member's behalf. Classifier answers in plain text on the ticket.
