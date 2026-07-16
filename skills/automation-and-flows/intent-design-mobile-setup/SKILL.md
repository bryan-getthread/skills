---
name: Mobile Setup Intent Design
description: Build the phone mail/app setup intent — point users at the client's end-user setup guides for the happy path, escalate enrollment and policy failures to a human. Use when asked to "build a mobile setup intent", "deflect phone email setup tickets", or when mobile setup ranks high in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets, search_knowledge_base]
---

# Mobile Setup Intent Design

Guide the admin through building a mobile-setup intent that serves the client's own end-user setup guide for the common case (mail/apps on a new phone) and recognizes the cases self-service cannot fix — MDM enrollment failures, conditional-access blocks, lost-phone MFA — and escalates them cleanly.

## When to use

- "Build an intent for setting up email on phones."
- "Every new phone generates a ticket that a KB article would have answered."
- Intent Mining flagged phone/mail setup as a deflectable request type.

## Design

**Trigger phrases (adapt to real ticket language):** "set up email on my phone", "new phone need email", "add work email to iphone", "outlook on my android", "install the email app", "work apps on my new phone", "phone email setup", "how do I get my email on my cell", "switching phones", "activate email on mobile".
Near-miss watch: "lost my phone" is a security event (MFA + possible wipe) — divert immediately; "email not working on my phone" when it previously worked is the email-issues intent; "need a phone" is a hardware request.

**Arguments to collect:**
- New phone or re-setup on an existing one; platform (iPhone/Android).
- What they need: mail only, or the client's standard app set.
- Whether the old phone is still available and working (MFA transfer hinges on this).
- Company-owned or personal device (BYOD vs. corporate policy paths differ).

**Reply flow (guide-first):**
1. `search_knowledge_base` at design time to confirm the client's end-user setup guide exists; the reply serves that guide (link + the two-line summary), matched to platform and BYOD/corporate.
2. If the old phone is gone or MFA prompts can't be received → do NOT attempt setup steps; this is an identity case — route to the verified human path (MFA re-registration is a credential action).
3. If the guide's steps fail with an enrollment/management error, or an "your organization requires..." block → ticket flagged "enrollment/conditional access" with the exact error text and device details; these are admin-side fixes.
4. Happy path completion → "did mail load?" — close as deflected on yes.

**Handoff rule:** MFA re-registration, MDM enrollment problems, and conditional-access blocks always go to a human. Lost/stolen phone reroutes to the client's security process before anything else.

**Variation hooks (per client):** the setup-guide article(s) per platform, BYOD policy and what personal devices may access, MDM product name for recognizing its error dialogs, standard app list.

**Success metric:** deflection rate on mobile-setup conversations (guide served, user confirms working, no ticket). Counter-metric: repeat contacts within a few days from the same user — the guide may be stale.

## Steps

1. `list_intents` — check for an existing mobile/phone-setup intent; prefer updating.
2. `search_knowledge_base` for the end-user setup guides. **If none exist, stop and say so** — this intent is a pointer to content; writing the guide comes first (offer it as a prerequisite task for the admin).
3. `search_tickets` for recent phone-setup tickets; mine phrasing and note which step users get stuck on (that step gets the two-line summary in the reply).
4. Draft the full spec: triggers, arguments, guide-first flow with the three escalation cases, variations. Test plan: 5 should-match, 3–5 should-not (lost-phone, mail-stopped-working, need-a-phone near-misses).
5. Show the draft; on explicit confirmation call `create_intent` then the variation tools.
6. Report what was created, restate the test plan, recommend activation after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, output the complete written spec for an admin to apply.
- Do not build this intent against guides that don't exist, and never let replies improvise setup steps — steps drift, guides get maintained.
- Lost/stolen device mentions divert to the security process immediately; MFA re-registration is a verified-identity action, never chat-resolved.
- BYOD replies must respect the client's policy on personal devices — do not encourage enrolling personal phones unless the variation says the client allows it.
- Confirm before any write; never activate on the member's behalf.
