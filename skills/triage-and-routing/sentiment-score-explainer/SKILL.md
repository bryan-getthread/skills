---
name: Sentiment Score Explainer
description: Explain why a ticket thread received its sentiment score by citing the exact messages that drove it — no hand-waving, no re-scoring.
category: Triage & Routing
tools: [search_tickets, run_assistive_ai, add_ticket_note]
---

# Sentiment Score Explainer

When a thread's sentiment score raises eyebrows ("why is this marked negative?"), this skill traces the score back to the specific messages and phrases that plausibly drove it, quoted verbatim.

## When to use

- "Why did this thread receive this sentiment score?"
- A flow fires when sentiment drops on a ticket and needs an evidence note for the tech.
- A lead reviews flagged-negative tickets and wants the driver messages, not a summary.

## Steps

1. Read the full thread with `search_tickets` — every message, in order, with authors and timestamps — plus the current sentiment score.
2. Identify the client-authored messages carrying sentiment signal: frustration, urgency, satisfaction, escalation language, repeated follow-ups, or sarcasm. Where useful, use `run_assistive_ai` sentiment analysis to cross-check your read.
3. For each driver, quote the exact phrase verbatim with its message position (author + timestamp). Distinguish frustration at the situation from frustration at the desk — the score usually reflects both, but techs need to know which.
4. Note counter-signals too: positive or neutral messages that pulled the other way, and whether the negative signal is recent or early-thread (recency usually dominates).
5. Output (or post as a plain-text note, if asked): the score, 2-4 quoted driver messages with one-line readings, counter-signals, and one line on whether the score looks consistent with the evidence.

## Guardrails

- Cite exact messages, quoted verbatim — never summarize a feeling into existence. Every claimed driver must be traceable to a quote.
- Explain the score; do not change it, and do not overwrite it with your own re-score. If the evidence contradicts the score, say "the score does not appear consistent with the thread" and leave it to a human.
- Only client-authored messages drive client sentiment; do not attribute a tech's tone to the client.
- Sentiment phrasing varies by language and culture; read messages in their own language before judging tone, and say so when the thread is non-English.
- This is read-only except for the optional note; no status, priority, or reply writes.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the internal note — plain text, no markdown. Format: `SENTIMENT <score>: driven by: [1] "<quote>" (<author>, <when>) - <one-line reading>. [2] ... Counter-signals: <one line or "none">. Consistency: <one line>.`
- If the thread has no client messages or the score is unavailable, output exactly: `SENTIMENT EXPLAINER: insufficient thread content to explain score.` and nothing else.
- Never modify the ticket; the note is the only write.
