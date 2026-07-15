---
name: Multilingual Reply
description: Draft the client reply in the client's own language — detect it from their messages, write natively in it, and provide an English back-translation so the technician can verify before sending.
category: Communication
tools: [search_tickets, view_openDraft]
---

# Multilingual Reply

For desks serving clients in more than one language: the client gets a natural reply in their language; the technician gets an English mirror to verify it says what they meant.

## When to use

- A client writes in Spanish, French, Dutch, German, Norwegian, or any non-English language and the reply should match.
- "Reply to this in their language."
- "Translate my reply to <language> before I send it."

## Steps

1. Detect the client's language from their messages in the thread — the language they actually write in, not their country or name. If the thread mixes languages, match their most recent substantive message; if genuinely ambiguous, ask the tech which language to use.
2. Draft the reply natively in that language — composed in it, not word-for-word translated from an English draft — applying the full house standards: Email Baseline Standard structure (communication/email-baseline-standard), client-reply voice rules (communication/client-reply), and locally natural register (formal/informal address per the client's own usage — match their vous/usted/Sie level).
3. Keep technical anchors unambiguous: product names, error messages, and ticket numbers stay as-is; error text the client must recognize on screen is quoted in its original language.
4. Provide the **English back-translation** directly below the draft, labeled "English back-translation (for your review)" — a faithful mirror of what the foreign-language draft actually says, including anything that shifted in translation, so the tech reviews meaning, not just vibes.
5. Present both together via view_openDraft (draft on top, back-translation below for removal before send) or in chat. The technician confirms before anything is sent.

## Guardrails

- The back-translation is mandatory whenever the tech doesn't read the target language — never ask them to approve text they can't verify.
- The two versions must match: never add nuance, promises, or softening in one that isn't in the other. If a concept doesn't translate cleanly, flag it ("<term> has no direct equivalent — I used X, which means Y").
- All standard client-reply guardrails apply across languages: no invented details, no jargon, defensive writing intact, reopen line included on close-outs (translated naturally).
- Remove the back-translation block before sending — it's for review only. Say so explicitly in the presentation.
- If the desired language uses honorifics or formality registers the thread doesn't reveal, default to formal.
- Degradation: view_openDraft is in-app only — over external MCP, output draft + back-translation in chat, clearly separated.
