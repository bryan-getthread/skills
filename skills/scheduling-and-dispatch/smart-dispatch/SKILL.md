---
name: Smart Dispatch
description: The composite flow-embedded dispatcher partners keep asking for — classify a new ticket, consult a routing matrix (tech specialties plus client familiarity from resolved-ticket history), then assign and schedule it in one unattended pass. Answers the top-voted "Dispatching Automation" request.
category: Scheduling & Dispatch
tools: [search_tickets, search_members, search_clients, list_boards, list_ticket_priorities, update_ticket, schedule_ticket, add_ticket_note]
---

# Smart Dispatch

End-to-end dispatch in a single pass: classify the ticket, score candidate technicians against a routing matrix, assign the winner, and put the work on their schedule. This is a composition skill — it chains the logic of `triage-and-routing/intake-classification-tree` (what is this ticket), `scheduling-and-dispatch/skill-based-routing` (who has demonstrated expertise), and `scheduling-and-dispatch/workload-balancing-assignment` (who has capacity) into one auditable decision.

## When to use

- "Automate our dispatching" — a Flow fires on ticket creation and the desk wants classify → route → assign → schedule with no human in the loop.
- A dispatcher wants one command that does the whole first-pass dispatch on a ticket instead of running triage, routing, and assignment separately.
- A desk wants dispatch decisions that weigh both *who knows this stack/client* and *who is least busy*, with the reasoning written down.

## Steps

1. **Classify.** Apply the intake-classification-tree logic to the ticket's summary and description: type (Incident/Request/Problem), affected technology, and priority sanity-check via `list_ticket_priorities`. If the ticket is unclassifiable (empty body, pure noise), stop — dispatch needs a classification.
2. **Build the routing matrix.** For each candidate technician from `search_members` (the relevant board's team, minus inactive members and stated exclusions), score two signals:
   - **Specialty fit:** does the tech's stated specialty (from the desk's configured specialty list encoded alongside this skill) match the classified technology?
   - **Client familiarity:** run `search_tickets` per candidate filtered to resolved tickets for this ticket's client — a tech who has closed several tickets for <client> recently scores higher. State result caps: if the search caps out, report familiarity as "at least N" rather than exact.
3. **Weigh capacity.** Apply the workload-balancing-assignment formula (base capacity − priority-weighted open tickets) to the top specialty/familiarity candidates so a perfect-fit tech who is drowning does not automatically win.
4. **Decide.** Pick the highest combined scorer. If the top two are effectively tied, or no candidate has both a plausible fit and capacity, do not guess — leave unassigned and post the score table for a human dispatcher.
5. **Assign and schedule.** `update_ticket` to set the owner, then `schedule_ticket` to place a work block sized to the classification (small default block unless the desk configured durations per ticket type).
6. **Record.** Post a plain-text internal note via `add_ticket_note`: the classification, the score table (winner, runner-up, formula inputs), and the scheduled block. No markdown, no emojis — this note may sync to a PSA.

## Guardrails

- **Honesty about calendars:** this skill is NOT capacity-calendar-aware — there is no Planner read on the tool surface, so it cannot see the tech's Outlook/meeting calendar. It schedules against Thread schedule entries only. When exact timing matters, pair with `scheduling-and-dispatch/calendar-aware-scheduling` in attended mode instead of trusting the scheduled block.
- Show the math. Every assignment carries its score breakdown so the dispatch is auditable, never a black box.
- Client-specific routing rules beat every score — if a rule names an owner or team for this client, follow it and say so in the note.
- Never assign to the requester, an inactive member, or an excluded member. Never reassign a ticket that already has an owner.
- Per-candidate `search_tickets` calls can hit result caps — split searches per member per board and report familiarity counts as minimums when capped.
- When in doubt (unclassifiable ticket, empty pool, tie, missing specialty data), do nothing beyond the diagnostic note. An unassigned ticket is recoverable; a mis-dispatched one costs a bounce.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the internal note — plain text, no narration, no questions, no markdown.
- Act only on a clear winner: single top scorer after exclusions, classification confident, client rule respected. Otherwise make no writes and post: "Smart dispatch: no unambiguous assignment (reason). Left for dispatcher." with the score table.
- Never touch status, priority, or anything other than owner, schedule entry, and the note.
- If the ticket already has an owner or an existing schedule entry, do nothing and post nothing.
