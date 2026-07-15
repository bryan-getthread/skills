---
name: Onboarding Guide Check-In
description: For the mentor or trainer of a new hire — "how is <new hire> doing": review their real recent tickets against the onboarding rubric and report progress versus the training plan.
category: Training & Enablement
tools: [search_tickets, search_members, notion-search, notion-fetch, notion-update-page]
---

# Onboarding Guide Check-In

The mentor-side companion to the onboarding coach. Reviews the new hire's actual recent ticket work against the same rubric used in training, compares progress to the plan, and gives the mentor a concrete picture: what is landing, what is not, and what to coach next.

## When to use

- "How is <new hire> doing?" / "check in on <new hire>'s progress"
- Weekly mentor check-ins during a ramp plan
- Before a 30/60/90-day review conversation
- "Is <new hire> ready for <next phase / harder queue>?"

## Steps

1. Resolve the new hire via search_members and confirm the review window (default: since last check-in, or the past 7 days).
2. Load the training plan if one exists (Notion when connected: the new hire's onboarding page) to know which phase they are in and what "on track" means this week. Without a plan, use a generic ramp expectation and say that is what you are doing.
3. Pull the new hire's tickets in the window with search_tickets: assigned, touched, and closed. Note volume and mix. If a search may have hit a result cap, disclose it rather than presenting counts as exact.
4. Review a sample (5–10 tickets, weighted toward closed and client-facing ones) against the onboarding rubric:
   - Diagnosis quality: right issue identified, sensible narrowing
   - Client communication: tone, expectation-setting, response gaps
   - Process hygiene: classification, status discipline, time logged, notes present
   - Independence: solved alone vs escalated vs sat idle
5. Compare against the previous check-in if history exists (Notion tracker or prior session summary): trending up, flat, or down, per rubric area.
6. Report to the mentor:
   - Headline: on track / ahead / needs attention, with the one-sentence reason
   - Per-rubric-area assessment with 1–2 concrete ticket examples each (ticket numbers, no client PII in the summary)
   - Progress vs plan: what this phase expected vs what was demonstrated
   - Suggested coaching focus for the next period (one or two things, not ten)
   - A readiness call if the mentor asked one ("ready for the VIP board? not yet — because X")
7. Offer to append the check-in summary to the tracking page (notion-update-page) so the next check-in has a baseline.

## Guardrails

- This is coaching support for the mentor, not covert performance surveillance — frame findings developmentally and stick to observable evidence from tickets.
- Cite real ticket numbers for every claim; never fabricate an example or generalize from a single ticket to a trait.
- Distinguish "new hire's gap" from "desk's gap": if their tickets stall because escalations go unanswered, say that.
- Notion plan/tracker access requires the mentor to have connected Notion; degrade gracefully to ticket evidence plus the mentor's stated plan.
- Small-sample honesty: if the window has only a handful of tickets, say the confidence is low.
