---
name: One-Shot Ticket Workup
description: Chain the assistive AI on an in-flight ticket — recap, suggested next step, drafted reply, and a drafted time entry — all as previews, nothing posted without confirmation. Answers the commonly requested "get me fully caught up and ready to act on this ticket" workflow; runs manually on demand.
category: Personal Productivity
tools: [run_assistive_ai, search_tickets, list_recap_templates]
---

# One-Shot Ticket Workup

Pick up any in-flight ticket and get everything you need to move it in one pass: what's happened (recap), what to do next (suggested step), a reply you can send, and a time entry for the work so far — all drafted as previews you approve or edit. This extends the new-ticket first-touch pattern to tickets already in motion.

## When to use

- "Work up this ticket for me" / "get me caught up and ready to act"
- "Recap, next step, a reply, and a time entry for #<n>"
- Returning to a ticket you haven't touched in days and need to re-engage fast
- Any in-flight ticket where you want the full assist in one shot, not four separate asks

## Steps

1. Load the ticket's full context with search_tickets / the ticket thread. Note where things stand: who has the ball, what's unresolved.
2. Chain run_assistive_ai to produce, in order, four **previews**:
   - **Recap** — what's happened and the current state (offer a recap template via list_recap_templates if the desk uses one).
   - **Suggested next step** — the single most useful next action, with a one-line why.
   - **Drafted reply** — a client-ready message for that next step, in the desk's tone.
   - **Drafted time entry** — a plain-text, PSA-safe summary of the work reflected on the ticket so far.
3. Present all four together, clearly labeled, so the member can read top-to-bottom and decide.
4. **Nothing is posted automatically.** For each item the member wants to act on, take it as a separate explicit confirmation, then perform that one write (post the reply, log the entry). Edits re-preview first.
5. If the member only wants some of the four, produce just those — don't force the full chain.

## Guardrails

- Preview-only until confirmed. No reply is sent, no note added, and no time logged without an explicit per-item go-ahead. This is the core promise of the skill.
- Ground every output in the ticket — the recap, next step, reply, and entry must reflect what the thread actually shows. No invented steps, outcomes, or facts.
- Keep the drafted time entry plain-text for PSA compatibility, and only reflect work actually evidenced.
- The suggested next step is a recommendation, never a completed action — never convert it into a write on its own.
- Member-invoked only; no unattended variant (a full interactive workup is member-driven, and Flows can't chain-and-confirm like this).
