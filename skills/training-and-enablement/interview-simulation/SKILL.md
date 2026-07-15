---
name: Interview Simulation
description: Run a structured technical-hire interview assessment — scenario-based questions matched to the role, a scoring rubric applied consistently, and a hiring-manager summary at the end.
category: Training & Enablement
tools: [search_tickets, search_knowledge_base]
---

# Interview Simulation

A structured technical interview for service-desk hiring. Scenario questions drawn from the kind of work this desk actually does, scored against a fixed rubric, ending in a written hiring-manager summary. Works for assessing a live candidate or letting an internal candidate practice.

## When to use

- "Run a technical interview for an L1 helpdesk candidate"
- "I'm interviewing someone in an hour — give me scenario questions and a rubric"
- An internal tech wants to practice interviewing for a promotion ("mock-interview me for the L2 role")
- Standardizing interviews so every candidate gets the same assessment

## Steps

1. Confirm the role and level (L1/L2/L3, dispatcher, etc.) and the mode: live-candidate assessment (the interviewer relays answers) or practice (the person answers directly).
2. Build the question set — 5–7 questions, grounded in this desk's real workload (infer common categories from search_tickets; keep all examples generic and sanitized):
   - 2–3 troubleshooting scenarios at the target level ("A user at a client site can print but not scan-to-email — walk me through your approach")
   - 1 prioritization/judgment scenario (two urgent things at once — what do they do and communicate?)
   - 1 client-communication scenario (explain a technical delay to a frustrated non-technical user)
   - 1 process/safety question (identity verification, when to escalate, what they never do without approval)
3. State the rubric before starting, and score each answer 1–5 on: technical soundness, structured thinking (narrowing vs guessing), communication clarity, and judgment/safety. Note that a great candidate says "I don't know, here's how I'd find out" — score that positively.
4. Ask one question at a time. For live mode, provide the interviewer with what a strong / acceptable / weak answer looks like for each question so they can probe.
5. Produce the hiring-manager summary:
   - Per-question scores with one-line evidence each
   - Overall score and level calibration ("performed at solid L1; L2 gaps in X")
   - Strengths, concerns, and suggested follow-up questions for a second round
   - A clear recommendation category: advance / advance-with-reservations / do-not-advance
6. In practice mode, add coaching notes: the model answer for each question they missed.

## Guardrails

- The rubric is fixed once the interview starts; score all candidates for a role against the same questions and scale.
- Assess only job-relevant skill and judgment. Do not evaluate, speculate on, or record anything about protected characteristics, and keep the summary strictly evidence-based ("said X", not personality diagnosis).
- The summary is an input to a human hiring decision, not the decision — say so in the output.
- Do not store candidate answers anywhere (no tickets, no notes); the summary lives in the conversation for the interviewer to copy.
- Never present invented "company policy" as real; where a question depends on desk policy (e.g., verification procedure), use the team's documented standard from the KB if it exists, otherwise a stated generic best practice.
