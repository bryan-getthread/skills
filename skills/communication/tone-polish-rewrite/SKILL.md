---
name: Tone Polish Rewrite
description: Rewrite rough technician text into a polished, client-ready version — "write this nicely," "make this better," "clean this up" — preserving every fact exactly.
category: Communication
tools: []
---

# Tone Polish Rewrite

Takes a technician's rough draft — terse, blunt, or hastily typed — and returns a polished version with identical factual content. The single most common drafting ask on a service desk.

## When to use

- "Write this nicely: <rough text>"
- "Make this better" / "make this sound professional"
- "Clean this up before I send it" — with a pasted draft.
- A message reads harsh, curt, or unclear and the tech wants a friendlier version.

## Steps

1. Read the rough text and identify every factual claim: what happened, what was done, times, names, commitments. These are immutable.
2. Rewrite for tone and clarity per the Email Baseline Standard skill (communication/email-baseline-standard): answer first, plain language, no filler, 1–2 short paragraphs unless the source demands more.
3. Honor any style constraints the member states, exactly — e.g. "no em-dashes" means zero em-dashes; "shorter" means shorter; "British spelling" means British spelling. Constraints stated once in a session apply to every rewrite in it.
4. Keep the member's meaning and commitments identical: do not soften a "no" into a "maybe," do not upgrade "should" to "will," do not add promises, apologies, or steps the source didn't contain.
5. Return **only the rewritten text** — no preamble, no "Here's a polished version:", no commentary, no options unless asked. The reply is the artifact; the member will copy-paste it.

## Guardrails

- Never add, remove, or alter facts. Polishing is tone-only; if the source is ambiguous, keep the ambiguity rather than guessing an interpretation.
- Never invent a greeting name, ticket number, or detail that isn't in the source text.
- If the source contains something that must not be sent to a client (credentials, internal pricing, blame), keep it out of the rewrite and add a single bracketed flag at the end: [Removed: <what> — internal only].
- Preserve the source language unless asked to translate (for non-native-English cleanup, see communication/esl-drafting-assistant).

## Unattended (Flows) variant

- Your entire reply is the rewritten text, verbatim — no narration, no labels.
- If the input is empty or contains no rewritable prose, output nothing.
