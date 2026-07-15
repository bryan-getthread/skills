---
name: Technical to Plain English
description: Translate a technical resolution, diagnosis, or explanation into language a non-technical stakeholder can understand — "explain this to the office manager," "make this client-friendly."
category: Communication
tools: [search_tickets]
---

# Technical to Plain English

Rewrites technical content for a non-technical reader: same truth, no jargon, framed around what it means for them rather than how it works.

## When to use

- "Explain this fix in plain English for the client."
- "The office manager asked what actually happened — translate this."
- Any resolution note, diagnosis, or vendor explanation that needs to leave the technical bubble.

## Steps

1. Read the technical source (resolution notes, diagnosis, pasted vendor response — pull from the ticket via search_tickets if referenced). Identify the three things the plain version must carry: what was wrong, what changed, and what it means for the reader.
2. Translate using these rules:
   - Replace every technical term with what it does, not a simpler synonym for what it is ("DNS wasn't resolving" → "computers couldn't look up where to find the server").
   - Analogies are allowed when they clarify, but keep them modest and accurate — an analogy that overstates certainty or severity is worse than jargon.
   - Lead with the reader's stake: impact and status first, mechanism second (and only as much mechanism as they need).
   - Keep quantities honest but round ("about 40 machines," not "38 endpoints").
   - Sentence-level test: would a smart person with no IT background nod along? If a sentence needs IT knowledge, rewrite it.
3. Preserve exactness where it matters: dates, durations, what data was or wasn't affected, and any action the reader must take stay precise, never approximated away.
4. Output the plain-English version standing alone. If it's destined for an email, apply the Email Baseline Standard (communication/email-baseline-standard); if the member just needs the explanation, the text itself is the artifact.

## Guardrails

- Simplify the language, never the truth: no rounding "mostly fixed" up to "fixed," no dropping caveats the technical version carried, no upgrading uncertainty into confidence.
- Never introduce cause claims the technical source doesn't make — plain English is not the place root-cause speculation sneaks in.
- Security topics keep defensive-writing rules (see communication/email-baseline-standard): "suspicious activity," not "hack," unless confirmed and approved language exists.
- Don't condescend: plain is respectful, not childish. No "the magic box that connects you to the internet."
- If the technical source is ambiguous or contradicts itself, flag the ambiguity to the tech instead of picking an interpretation for the client.
