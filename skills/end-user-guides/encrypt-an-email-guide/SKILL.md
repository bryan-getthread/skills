---
name: Encrypt an Email Guide
description: Draft reply-ready instructions for an end user to send an encrypted or sensitive email using the client's actual encryption method — "tell the user how to send this securely."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Encrypt an Email Guide

Produces a client-ready instruction block for a user who needs to send something sensitive by email securely, written for the specific encryption method the client actually runs — there is no generic "encrypt an email," and the trigger word/method differs at every shop.

## When to use

- "User needs to email a client something confidential — send them how to encrypt it."
- "How do I send a secure email with patient/financial info?"
- Compliance-driven tickets where a message must go out protected.

## Steps

1. **Identify the client's encryption method first** (search_itglue / search_hudu / search_knowledge_base / search_tickets). Methods differ completely: Microsoft Purview Message Encryption (often triggered by an **Encrypt button** in Outlook or by typing a keyword like `[secure]` in the subject that a mail rule catches), a third-party gateway (Zix, Barracuda, Proofpoint, Virtru, etc.), or a secure portal upload. Note the exact trigger and what the recipient will experience. If the method is unknown, ask the technician ONE question — sending "just click Encrypt" to a shop without that button teaches the user the desk is guessing.
2. **Confirm the subject-keyword branch if that's the mechanism.** If the client uses a subject-line keyword rule, the exact keyword and its placement matter — pull it from docs verbatim; never invent a keyword.
3. **Draft the instruction block** to end-user rules, one action per step with what-you'll-see cues:
   - The trigger, exactly: the Encrypt/Protect button location (Options tab or the compose toolbar) OR the precise subject keyword to type, with a cue confirming it took ("you'll see an 'Encrypt' banner on the message").
   - What the recipient sees, so the user can reassure them: often a notification email with a "read the message" link and a one-time passcode — "that's normal, it proves it's really them."
   - The sensible-habits line: put the sensitive content in the encrypted message body/attachment, not in the subject line (subjects usually aren't encrypted).
   - The success confidence check appropriate to the method (banner present, or a documented confirmation).
   - Off-ramp: "If you don't see the Encrypt option, stop and reply — don't send it unprotected — we'll sort out how it's set up for you."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Method and trigger match are mandatory.** A Purview "Encrypt button" walkthrough sent to a Zix-gateway shop, or a wrong subject keyword, fails silently — and silent failure with sensitive data is the worst outcome.
- **Never invent a subject keyword, portal URL, or button name** — docs or ask are the only sources.
- The "don't put sensitive info in the subject line" note stays in every draft.
- The "if you can't find it, don't send it unprotected" off-ramp is non-negotiable — better a delay than an exposure.
- This guide does not render a compliance verdict on whether a given item is allowed to be sent at all — that's a policy call above the guide; if the user is unsure, they reply first.
- No admin steps (encryption rules, gateway policy, DLP config) in the user block.
- Localizable; version-cautious UI cues.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- end-user-guides/safe-file-sharing-guide — when the sensitive item is a file better sent as a protected link than an attachment.
- end-user-guides/large-file-share-guide — when it's also too big for email.
- communication/email-baseline-standard.
