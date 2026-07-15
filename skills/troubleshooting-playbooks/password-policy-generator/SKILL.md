---
name: Password Policy Generator
description: Generate a compliant, memorable password or passphrase to a stated policy (length, character classes, banned patterns) — with delivery only over a secure channel, never stored, never placed in tickets or email.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note]
---

# Password Policy Generator

Techs need "a password that passes <client>'s policy" a dozen times a week — for resets, new accounts, service accounts, Wi-Fi. This skill produces one that genuinely satisfies the stated policy and is memorable where a human must type it, and enforces the part that matters more than the string: how it is (and is not) delivered and recorded.

## When to use

- "Generate a password that meets <client>'s policy" during a reset or new-account setup
- "I need a passphrase for <user> — 14+ chars, upper/lower/number/symbol"
- Wi-Fi/shared-resource passphrases to a stated standard
- "Does this policy allow X?" sanity checks while generating

## Steps

1. **Get the policy — never assume it.** From the requester or from search_knowledge_base / search_itglue / search_hudu (clients document password standards): minimum length, required character classes, banned content (username, company name, previous-password similarity), expiry/rotation context, and where it will be used (systems that choke on certain symbols exist — ask if the target system has character restrictions).
2. **Choose the form for the use.**
   - **Human-typed, human-remembered** (user accounts): a passphrase — several unrelated words with the policy's required classes worked in (capitalization, a number, a separator symbol), comfortably over the minimum length. Memorable beats maximal complexity for humans; length carries the strength.
   - **Machine-stored** (service accounts, Wi-Fi PSKs going into config, anything living in a password manager): a long random string — no need for memorability, so use it; 20+ characters or the policy's max comfort.
   - **Temporary/first-login** (resets where the user must change at next logon): a passphrase easy to convey by voice without ambiguity — avoid characters that are misheard or mistyped (O/0, l/1/I, ambiguous symbols) since it will be read over a callback.
3. **Generate fresh, check compliance explicitly.** Produce a new value every time — never reuse a previously generated one, never use examples from documentation or this skill, and never derive from the user's name, the client, the season, or the year (Autumn2026! is the pattern attackers try first and many policies now ban). Verify the result against each policy requirement one by one and state the checklist ("14+ ✓, upper ✓, ...") so the requester sees compliance, not vibes.
4. **Deliver over a secure channel only.** The generated value goes to the requester through the client's documented secure practice: a one-time secret link, the password manager's sharing feature, or read aloud on a verified call. It does NOT go in: the ticket, ticket notes, email, chat that syncs to the PSA, or this conversation's summary. If the requester asks for it in the ticket "for convenience," decline and offer the secure route — that is the point of the skill.
5. **For resets, keep the identity ladder intact.** Generating the password does not skip verification: the requesting user is verified per the client's practice (callback to a number on file — never a number from the ticket) before any reset is performed with this value, and temporary passwords are set to require change at first use wherever the platform supports it.
6. **Record the event, not the secret.** add_ticket_note in plain text: that a compliant credential was generated, which policy it was checked against, how it was delivered (channel), and that it was not recorded — never the value itself, in full or in part, nor hints ("three words about weather").

## Guardrails

- NEVER store, log, or write the generated value anywhere persistent — not in ticket notes, emails, documentation, or summaries; the secure delivery channel is the only place it exists.
- Secure-channel delivery is non-negotiable: refuse plaintext-in-ticket/email requests and provide the alternative; if no secure channel exists at this client, that gap is the finding to escalate.
- Always generate fresh; never reuse values across users or tickets, and never emit dictionary-of-the-desk patterns (Company+Year!, Welcome1!, seasons) even if the policy would technically accept them.
- Temporary credentials require change-at-first-login wherever supported, and resets still require identity verification by callback to a number on file.
- If the stated policy is dangerously weak (e.g. 8 chars, no ban on common patterns), still comply — but note the weakness to the requester as a policy finding for the client conversation, don't silently exceed or ignore the request.
- This skill produces and delivers a value; it performs no resets itself — the reset action belongs to the tech under the identity-verification ladder.
