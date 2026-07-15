---
name: Super Magic Enablement
description: Prep a "show and tell" of the highest-value Super Magic use cases for THIS team — grounded in their own recent tickets, so every example is one the audience recognizes.
category: Training & Enablement
tools: [search_tickets, search_thread_docs, search_members]
---

# Super Magic Enablement

Builds a team-specific Super Magic demo plan. Instead of a generic feature tour, it mines the team's own recent tickets to find where Super Magic would have saved the most time this month, and packages the top use cases as demo-ready examples.

## When to use

- "Help me show the team what Super Magic can do"
- A lead is running a lunch-and-learn or all-hands segment on AI adoption
- "Which Super Magic features would help us most?"
- New team members need a practical intro grounded in their actual work

## Steps

1. Confirm the audience (whole desk, dispatchers, a specific pod) and session length (default: 20 minutes, 4–5 use cases).
2. Mine the team's recent tickets (search_tickets, last 30 days) for the patterns Super Magic serves best, and estimate rough frequency for each — disclose result caps rather than exact-sounding counts:
   - Repetitive drafting (many similar client replies) → reply drafting and tone polish
   - Long multi-touch threads → summarize-and-next-steps
   - "Has this happened before?" digging → similar-ticket and history search
   - Drive-by capture (work done, ticket created after) → create-ticket-with-time-entry by chat
   - Queue questions → daily digest / what-should-I-work-on
   - Repeat manual routines → candidates for skills and Flows
3. For each selected use case, build a demo card:
   - The recognizable pain: "Last month you had roughly N tickets like <sanitized example>"
   - The exact prompt to type, word for word
   - What the audience will see happen
   - The time saved estimate, stated conservatively
4. Order the cards for the session: open with the most universally felt pain, put the most impressive-looking demo second, end with one "builder" example (a skill or Flow) as the where-this-goes moment.
5. Verify each demo prompt is safe to run live: prefer read-only demos (summaries, digests, searches) on real tickets, and use a test or sandbox ticket for any write demo (notes, time entries, status changes).
6. Output the session plan: agenda with timings, the demo cards, a one-slide-equivalent summary of "what to try this week" (3 prompts per role), and speaker notes on likely questions. Reference official capability descriptions from search_thread_docs where precision matters.

## Guardrails

- Every use case must trace to observed ticket patterns from this team — no generic marketing claims presented as their data.
- Time-saved figures are estimates; label them as such and keep them conservative.
- Live-demo safety: never script a write action against a real client ticket in the session plan; call out which demos mutate data.
- Sanitize all example tickets shown to a group (placeholder client and user names).
- If a capability question goes beyond what search_thread_docs confirms, say "verify with Thread docs/support" rather than overpromising a feature.
