---
name: Lunch and Learn Planner
description: Plan internal enablement sessions from the desk's own recent tickets — mine the last month for genuinely teachable resolutions, rotate presenters fairly, and package each session into a 30-minute format. Use for "we should do lunch-and-learns" or "what should this month's session cover?".
category: MSP Business Operations
tools: [search_tickets, search_members, search_knowledge_base, create_ticket, schedule_ticket, add_ticket_note]
---

# Lunch and Learn Planner

Turns the desk's most interesting recent work into a lightweight internal training program: topics mined from real resolved tickets (the weird fix, the new client technology, the escalation that taught everyone something), a presenter rotation that spreads the stage around, and a 30-minute format that respects lunch. The output is a session calendar plus a ready outline per session.

## When to use

- "Set up a monthly lunch-and-learn program for the team."
- "What should this month's session be? Find something interesting from recent tickets."
- "<user> solved something gnarly last week — turn it into a session."
- Refreshing a program that's gone stale ("we keep doing the same three topics").

## Steps

1. Confirm the shape with the organizer: cadence (monthly default; biweekly for bigger teams), audience (whole desk vs a level), and whether sessions are recorded/notes-captured for the KB.
2. Mine topics from the last 30–60 days of resolved tickets (search_tickets), looking for the four teachable shapes:
   - **The clever fix** — a resolution that took real diagnostic work and would save the next person hours.
   - **The new thing** — a technology that just entered the client stack and most of the team hasn't touched.
   - **The recurring burn** — an issue family that keeps coming back; a session beats ten more tickets (check the KB first — if a good article already exists, the session is "here's the article, here's the demo").
   - **The near-miss** — a process lesson (an escalation that went sideways, a change that needed rollback), taught blameless: the session is about the failure mode, never the person who hit it.
3. Shortlist 3–6 topics and confirm with the organizer. For each, identify the natural presenter — usually the tech who resolved it — and check the rotation: track who has presented recently so the same two confident voices don't own every slot, and quieter members get low-stakes slots (co-presenting counts, and a 10-minute segment is a fine first stage).
4. Ask, don't assign: presenting is invited, not conscripted. Draft the invitation note for the organizer ("your <topic> fix last month was great — would you walk the team through it for 15 minutes?") and offer prep help.
5. Build the 30-minute outline per session and post it to the session's ticket:
   - 5 min — the setup: the ticket's symptom as the team first saw it (sanitized: <client>, <user>).
   - 12 min — the walkthrough: diagnosis path including the dead ends (the dead ends are the teaching).
   - 8 min — demo or hands-on where feasible, or "how you'd recognize this next time".
   - 5 min — Q&A + capture: one person writes the takeaways into the KB article draft.
6. Schedule the series (create_ticket per session on the internal board, schedule_ticket for the slot), and close each session's loop afterward: takeaways note posted, KB draft filed, next session's topic confirmed.

## Guardrails

- Blameless always: near-miss sessions describe the failure mode in process terms; the tech involved volunteers the story or it isn't told. Never turn a session into a public retrospective on someone's mistake.
- Sanitize session materials — client names, credentials, and screenshots with identifying data stay out; the lesson survives sanitization or it isn't a lesson.
- Presenter rotation is fairness infrastructure, not a duty roster: track and encourage breadth, but a decline is a complete answer and never noted anywhere.
- Keep it genuinely 30 minutes and genuinely optional-lunch-friendly; a program that respects time survives, one that sprawls dies by month three.
- Verify technical content in outlines against the actual ticket resolution and current vendor docs — a session that teaches the wrong fix is worse than no session.
- This skill plans and drafts; the organizer invites, books the room/call, and runs it. Cross-reference training-video-outline (training-and-enablement) when a session is worth recording as standing material.
