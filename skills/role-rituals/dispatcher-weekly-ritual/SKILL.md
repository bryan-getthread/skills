---
name: Dispatcher Weekly Ritual
description: A dispatcher's weekly deep pass — dispatch control tower, fixed-assignee rule review, skill-based-routing accuracy check, and schedule-density look-ahead — run when a dispatcher says "weekly dispatch review" or "tune the routing".
category: Role Rituals
tools: [search_tickets, search_members, list_boards]
---

# Dispatcher Weekly Ritual

The weekly step-back from daily firefighting: audit how routing actually performed this week and tune the rules, so next week's morning rituals get easier. ~45 minutes, once a week on a quiet stretch.

## When to use

- "Run the weekly dispatch review" / "how did routing do this week" / "tune the assignment rules."
- After a week with visible mis-assignments or bounced tickets.
- Prep for a routing-rules conversation with the service manager.

## Steps

1. **Control-tower deep pass (15 min)** — run `skills/scheduling-and-dispatch/dispatch-control-tower` over the full week: load distribution per tech, bounce rate, time-to-assignment, boards that ran hot.
2. **Fixed-assignee rule review (10 min)** — audit standing rules using `skills/scheduling-and-dispatch/fixed-assignee-alert-routing` as the lens: did any fixed assignee become a bottleneck, go on leave, or receive volume outside their lane? Propose rule changes; do not silently change them.
3. **Skill-based-routing accuracy (10 min)** — sample this week's routed tickets against `skills/scheduling-and-dispatch/skill-based-routing`: how many landed on someone who then reassigned for skill reasons? List the mismatch patterns (e.g., a ticket type consistently mis-mapped).
4. **Schedule-density look-ahead (10 min)** — run `skills/scheduling-and-dispatch/technician-availability-check` across next week: PTO collisions, days already >80% booked, field-visit clusters. Flag days where new urgent work will have nowhere to land.
5. Output the weekly dispatch review: this week's routing scorecard, proposed rule changes (as proposals), mismatch patterns, and next week's density warnings.

## Time-short version

Steps 1 and 4 — know what happened and what's about to be tight. Rule tuning (2 and 3) can slip a week with a note; density collisions cannot.

## Guardrails

- Rule changes are proposals until a human approves them — never edit standing routing rules as a side effect of a review.
- Weight patterns by count, not anecdote: one bounced ticket is noise, five to the same tech is a pattern.
- Per-tech load numbers get shared carefully — this review is capacity planning, not a leaderboard.
- Skip-with-note beats fake completion; disclose result caps on weekly-volume searches.
