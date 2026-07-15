---
name: Product Knowledge Quiz
description: Quiz a technician on the team's stack, tools, or product curriculum — phase-aware, understanding-style questions (predict the outcome, spot the mistake), with gap reinforcement at the end.
category: Training & Enablement
tools: [search_knowledge_base, search_tickets, search_thread_docs, notion-search, notion-fetch]
---

# Product Knowledge Quiz

An interactive quiz on the technologies and processes this desk actually supports. Questions test understanding — predictions, spot-the-mistake, "what breaks if…" — not trivia recall. Ends with a score and targeted reinforcement of whatever the tech missed.

## When to use

- "Quiz me on <topic>" (M365, networking, the ticketing process, a product line)
- A trainer says "test <new hire> on phase 2 of the curriculum"
- "Am I ready to take <topic> tickets?"
- Weekly knowledge check-ins during onboarding

## Steps

1. Establish scope. If the team keeps a training curriculum (Notion when connected, or the knowledge base), find the phase the tech is on and quiz that phase only. Otherwise infer the desk's real stack from recent tickets (search_tickets) and KB articles (search_knowledge_base) and confirm the topic with the tech before starting.
2. Build 5–8 questions weighted toward understanding, not definitions:
   - Prediction: "A user's mailbox is converted to shared and the license is removed. What happens to their delegated calendar access?"
   - Spot-the-mistake: show a short (invented, clearly labeled) troubleshooting sequence with one wrong step; ask which step is wrong and why.
   - Ordering: "Put these offboarding steps in the safe order."
   - Scenario judgment: "Client reports X — what do you check first, and what do you NOT touch?"
3. Ask one question at a time. Wait for the answer before showing the next.
4. Grade each answer immediately: correct/partial/incorrect, with a one-sentence explanation of the right answer. Partial credit is fine — say what half they got.
5. After the last question, give: score, the 1–2 weakest areas, and for each weak area a short reinforcement (the correct mental model in 3–4 sentences plus a pointer to the relevant KB article or Thread doc if one exists).
6. Offer a follow-up: "Want 3 more questions on just the weak area?"

## Guardrails

- Ground questions in the team's actual environment and documentation where possible; when you invent a scenario, make it generic and plausible — never present an invented client situation as a real ticket.
- Do not fabricate KB articles or doc links for reinforcement; if no source exists, say the topic is undocumented (that itself is useful signal for the trainer).
- Quiz difficulty follows the tech's phase; do not quiz phase-3 material at phase 1 unless asked.
- Curriculum lookup in Notion requires the member to have connected Notion; degrade to KB/ticket-inferred topics otherwise.
- This is a learning tool, not surveillance: share results with the tech in the session; only report to a trainer if the trainer initiated the quiz or the tech agrees.
