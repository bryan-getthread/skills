---
name: First Touch Quality Report
description: Someone wants to know how often the first reply to a client was a real, substantive response versus a holding "we're looking into it" placeholder.
category: Reporting & Analytics
tools: [search_tickets, list_boards, list_ticket_priorities, list_ticket_statuses]
---

# First Touch Quality Report

Look past first-response *speed* to first-response *substance*: what share of first replies actually moved the ticket forward (answered, asked a diagnosing question, gave next steps) versus merely acknowledged receipt. A fast holding reply games the response-time metric without helping the client.

## When to use

- "Our response times look great — but are the first replies actually useful?"
- Auditing whether a fast-first-response push produced empty placeholder replies.
- Coaching input on first-contact quality across the team or a board.

## Steps

1. Confirm the period, boards in scope (list_boards), and the definition of a substantive first touch: it does at least one of — answers the question, requests specific diagnostic info, provides a concrete next step or workaround, or sets a specific expectation. A generic "thanks, we're on it" with no substance is a holding reply. State this rubric up front.
2. Assemble the population of tickets with a first outbound reply in the period (exclude automated acknowledgements). Run split searches per board and per priority (result-cap honesty; record caps). This report reads thread content, so work from a bounded, sampled set where the volume is large — and say it is a sample.
3. Classify each first touch as substantive vs holding against the rubric, reading the first reply text. Count anything genuinely ambiguous as unclassified rather than forcing it.
4. Report the substantive-first-touch rate overall, then sliced by board and priority — holding replies on high-priority tickets are the worst offense and worth isolating.
5. Pair with speed if available: a fast median first response with a low substance rate is the exact anti-pattern this report exists to expose.
6. Output: the substantive rate (with denominator and sample note), the board/priority slices, the speed-vs-substance contrast, a few de-identified illustrative examples of each class, and a methodology note (rubric, sampling, exclusions, boards, period, caps).

## Guardrails

- State the substantive-vs-holding rubric explicitly; the whole report is a judgment call and must be transparent and consistent, not vibes.
- Be honest about sampling — reading thread content at scale means this is usually a sample, not a census; give the sample size and never present it as exact.
- This is a coaching and process signal, not a leaderboard; aggregate patterns, role-aware framing, and do not rank named techs by substance rate alone.
- Illustrative examples must be de-identified (no client, contact, or tech names).
- Never present capped or sampled searches as complete — disclose.
- Do not invent reply content or classifications; ambiguous first touches are unclassified.
