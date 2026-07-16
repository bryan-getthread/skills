---
name: Password Reset Intent Design
description: Build the password-reset intent — the highest-volume deflection target on most desks — with an SSPR-first reply path and a strict identity-verification handoff. Use when asked to "build a password reset intent", "deflect password tickets", or when Intent Mining ranks password reset as a top candidate.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets, search_knowledge_base]
---

# Password Reset Intent Design

Guide the admin through building a password-reset intent that pushes users to self-service reset (SSPR) first and routes everything else to a verified human handoff. The intent NEVER resets a password itself — it deflects to SSPR or produces a well-formed, identity-flagged ticket.

## When to use

- "Build an intent for password resets."
- "Half our tickets are password resets — can Magic handle them?"
- Intent Mining ranked password reset as the top build candidate.

## Design

**Trigger phrases (adapt to real ticket language):** "forgot my password", "reset my password", "can't log in", "cant login", "locked out of my account", "password expired", "my password isn't working", "account locked", "pw reset", "need a new password", "login not working", "I keep getting invalid password".
Watch for collisions: "can't log in to <specific app>" may belong to an application-specific intent — keep these triggers generic to account/directory sign-in.

**Arguments to collect:**
- Which system or account (directory/M365 sign-in vs. a specific application) — routes the reply.
- Whether they can still receive MFA prompts / access their registered recovery method — decides SSPR vs. handoff.
- Device context only if the client's SSPR flow differs by device (variation hook).

**Reply flow (SSPR-first ladder):**
1. If the account is directory/M365 and the user can use their registered recovery method → reply with the client's SSPR link and the exact steps, then ask "did that work?"
2. If SSPR succeeded → confirm resolution, close as deflected.
3. If SSPR failed, the user has no recovery method, or the system has no SSPR → create a ticket flagged "identity verification required" with the collected arguments, and tell the user a technician will verify their identity before any reset.

**Handoff rule (non-negotiable):** any path that would end in a human changing a credential must state that identity verification happens first. The intent never communicates a password, never resets one, and never bypasses MFA.

**Variation hooks (per client):** SSPR portal URL, which identity provider, whether SSPR is enabled at all (if not, the variation skips straight to the verified-ticket path), client-specific verification policy wording.

**Success metric:** deflection rate on password-reset conversations — matched conversations that end without a ticket. Track weekly; a healthy SSPR-enabled client trends toward the majority deflected. Also watch false-match rate on the near-miss test set.

## Steps

1. `list_intents` — if a password or login intent already exists, propose updating it (`get_intent` → `update_intent`) rather than duplicating.
2. `search_tickets` for 5–10 recent real password/lockout tickets; mirror the users' actual phrasing in the trigger set and note which systems come up.
3. `search_knowledge_base` for an existing SSPR article; if found, reference it in the reply instead of restating steps that may drift.
4. Draft the full spec (triggers, arguments, reply ladder, handoff wording, variations) and a test plan — 5 utterances that should match, 3–5 near-misses (e.g. "reset the printer", "password for the wifi") that should not. Show it before any write.
5. On explicit confirmation: `create_intent`, then `set_variation_arguments` / `set_variation_replies` / `update_variation` per client variation.
6. Report what was created, restate the test plan, and recommend the admin activate only after the test utterances pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent from the tool list, output the complete written spec for an admin to apply instead.
- Credential actions ALWAYS hand off to a verified human path — never design a reply that reveals, sets, or resets a password, or that walks a user around MFA.
- Do not invent the client's SSPR URL or verification policy; if unknown, leave a placeholder and flag it as required before activation.
- Replies are customer-facing: plain, localizable language, no internal jargon, no fabricated turnaround promises.
- Never create or update without showing the complete draft and getting explicit confirmation; never activate on the member's behalf.
