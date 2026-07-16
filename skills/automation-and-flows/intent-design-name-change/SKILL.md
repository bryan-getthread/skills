---
name: Name Change Intent Design
description: Build the name-change intent — capture the old and new name, the reason/effective date, and every system the change touches so the ticket arrives complete and nothing gets missed. Use when asked to "build a name change intent", "handle legal/display name updates", or when name changes show up in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets]
---

# Name Change Intent Design

Guide the admin through building an intent for user name changes (legal or display). The value is completeness: a name change touches many systems, and a half-filled ticket leaves the user with a mismatched email here and an old display name there. This intent does not deflect — it captures the change and the full list of places it must propagate.

## When to use

- "Build an intent for name changes."
- "Name-change tickets always miss a system — email says one name, Teams shows another."
- Intent Mining flagged name/display-name updates as a recurring request.

## Design

**Trigger phrases (adapt to real ticket language):** "I changed my name", "update my last name", "legal name change", "got married, need my name updated", "change my display name", "my email still shows my old name", "update my name in the system", "preferred name change".
Near-miss watch: "new hire" belongs to the onboarding intent; "someone is leaving" belongs to offboarding — this is scoped to an *existing* user's name change.

**Arguments to collect:**
- Old name (as currently in the systems) and new name (exact spelling).
- Type: legal name change vs. display/preferred name only (affects whether email/UPN changes).
- Whether the email address / username should change too, or only the display name — and if the old address should still receive mail (alias).
- Effective date, if not immediate.
- Which systems are in scope (directory/M365, email display + alias, PSA/HR contact record, chat/Teams, LOB apps, distribution lists, signatures, licenses tied to the account).
- Who authorized it (HR or manager) if the client requires it.

**Reply flow:**
1. Collect the arguments; confirm old→new name and whether the email address itself changes.
2. Create the ticket with a structured plain-text block that enumerates every in-scope system as a checklist, and route to the identity/M365 workflow.
3. Reply that the change will be applied across systems and that some propagation (mail routing, cached directories) can take time — do not promise an instant cutover.

**Handoff rule:** renaming accounts, changing UPNs/email, and updating HR records are human/workflow actions — the intent performs intake and produces the checklist, not the changes. If a legal change requires HR documentation, state that.

**Variation hooks (per client):** the exact system checklist (which LOB apps carry names), whether email/UPN changes are allowed or display-only, alias-retention policy, HR authorization requirement, which board or flow receives the ticket.

**Success metric:** first-touch completeness — percentage of name-change tickets that reach the technician with old/new name, email-change decision, and the full system checklist captured (zero follow-up questions). Secondary: rate of "you missed system X" reopens.

## Steps

1. `list_intents` — check for an existing name-change/identity intent; prefer `update_intent` over a duplicate.
2. `search_tickets` for past name-change tickets; mine phrasing (triggers) and — crucially — the systems that got missed and had to be reopened (those define the checklist arguments).
3. Draft the full spec: triggers, arguments (with the system checklist), confirmation reply, routing target, variations, and a test plan (5 should-match, 3–5 should-not including a new-hire near-miss). Show before any write.
4. On explicit confirmation: `create_intent`, then `set_variation_arguments` / `set_variation_replies` / `update_variation`.
5. Report what was created, restate the test plan, recommend activation only after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, degrade to a complete written spec for an admin to apply.
- The intent captures the change and the checklist — it does not rename accounts or edit records; those follow the client's identity workflow.
- Do not invent the client's system list or whether email/UPN changes are permitted — placeholder anything unknown and flag it before activation.
- Never promise an instant, fully-propagated cutover; note that mail routing and cached directories take time.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text (PSA-safe, no markdown).
