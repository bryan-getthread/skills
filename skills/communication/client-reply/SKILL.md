---
name: Client Reply
description: Draft an external client reply in your house voice and format — resolution updates, status notes, closing messages, or any client-facing email on a ticket.
category: Communication
tools: [search_tickets, view_openDraft, add_ticket_note]
---

# Client Reply

**When to use:** A technician needs to send a client-facing message and wants it on-brand and ready to send.

**What you get:** A drafted reply that leads with the answer, matches your house voice standards, and is safe to send as-is.

## When to use

- "Draft a reply to the client on this ticket."
- "Tell <user> their account is unlocked."
- "Write a response letting them know we're still working with the vendor."
- Any client-facing email where tone, format, and accuracy matter.

## Steps

1. Read the full ticket thread before writing a word — every prior message, note, and status change. Never draft from the latest message alone.
2. Draft the reply, leading with the answer or the current state, then the supporting detail.
3. Apply the house voice standards:
   - Greet the client by first name on the **first** message of a thread only; skip the greeting on subsequent replies in the same thread.
   - No filler openers ("I hope this email finds you well", "Just reaching out"). Start with substance.
   - No technical jargon unless the client used the term first; if a technical term is unavoidable, add a plain-language gloss.
   - 1–2 short paragraphs. If it needs more, it probably needs a call.
   - Structure per the Email Baseline Standard skill (communication/email-baseline-standard).
4. If the member has a preferred/banned word list configured (a personal-context skill or house style note), apply it: replace banned words with their designated substitutes; prefer listed alternatives.
5. If this is a close-out reply, end the body with the reopen line: "If anything isn't resolved, just reply to this email — replying reopens this ticket."
6. Signature: if the workspace applies a managed signature automatically, end after the last body sentence — do NOT add a closing sign-off ("Best regards, ...") that would double up. Only add a sign-off when no managed signature exists.
7. Present the reply as an open draft with view_openDraft for the technician to review. Do not send.

## Guardrails

- Never invent details: no made-up timestamps, steps taken, ticket numbers, links, or promises. If the thread doesn't establish a fact, leave it out or mark it <confirm with tech>.
- Keep the draft as a draft until a human approves it. Never send autonomously.
- Never expose internal notes, credentials, pricing, or other clients' details in a client-facing reply.
- Match the client's language: if the thread is in another language, draft in that language (see communication/multilingual-reply for the back-translation workflow).
- Degradation: view_openDraft is available in-app only. Over external MCP, output the finished draft in chat clearly labeled "DRAFT — review before sending" instead.

## Consolidates

Write/send email response, standard client reply format, reply-in-a-named-voice, tone, banned-word, and signature skills.
