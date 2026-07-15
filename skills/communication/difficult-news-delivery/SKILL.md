---
name: Difficult News Delivery
description: Draft the message no one wants to send — data loss, an unrecoverable state, a security exposure — factual, empathetic, and pointed at the path forward, under strict defensive-writing rules.
category: Communication
tools: [search_tickets, view_openDraft]
---

# Difficult News Delivery

For the worst emails on a service desk: the ones announcing that something is lost, exposed, or not coming back. Clarity and honesty, without a word that makes a bad situation legally or emotionally worse.

## When to use

- "The backups don't cover it — the data is gone. Help me tell the client."
- "We have to tell them their mailbox was exposed / the drive is unrecoverable / the project data is corrupted."
- Any message delivering a confirmed, materially bad outcome to a client.

## Steps

1. Verify with the technician that the bad outcome is **confirmed** — recovery genuinely exhausted, exposure genuinely established. This message is irreversible in effect; if any avenue is still open, the honest email is a status update (communication/delay-apology or client-reply), not this one.
2. Confirm who should deliver it and how. News of this weight often warrants a call first with the email as the written follow-up — recommend that, and check whether management or the account manager must review before anything is sent. For confirmed security incidents, incident-response or legal language may govern; if such requirements exist, they win.
3. Draft per the Email Baseline Standard skill (communication/email-baseline-standard), with the defensive-writing rules at maximum:
   - **The news, early and plainly.** No burying it under paragraph three: "I have difficult news about <the data/system>." Then the fact, stated exactly — what is lost or exposed, and the confirmed scope. Precision is kindness here: "the files created between <date> and <date> on <share>" beats "some data."
   - **What was done.** The recovery/containment attempts, factually — this shows diligence without pleading for credit.
   - **Empathy, one genuine beat.** Acknowledge what this means for them, without melodrama.
   - **The path forward.** Concrete next steps: what can be reconstructed, what protective actions to take (password resets, monitoring), what the desk will do next, and when they'll hear from us. The email must end in motion.
4. Scrub the draft against the banned patterns below, then present via view_openDraft with an explicit "review with your manager before sending" recommendation.

## Guardrails

- **Facts only, scope only.** State what is confirmed; never speculate on cause, fault, or blame — not the client's, not a vendor's, not the desk's. Admissions of fault or liability are management/legal decisions, never a draft default.
- **Defensive vocabulary:** no "breach," "hacked," "negligence," "our fault," "catastrophic," or guarantees ("this will never happen again"). For exposure events: "unauthorized access to X was confirmed" — exactly what is known, nothing more.
- Never soften into falsehood: no "some data may be affected" when the loss is confirmed, no implying recovery hope that doesn't exist. Kindness through clarity, not through fog.
- No compensation, remediation-cost, or contractual statements in the draft.
- This draft always gets human review — recommend manager sign-off explicitly. Never send autonomously under any configuration; this skill has **no unattended variant** by design.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft in chat labeled "SENSITIVE DRAFT — manager review required."
