---
name: Workload-Balancing Assignment
description: Assign a ticket to the technician with the best availability score — a transparent formula (base capacity minus open tickets minus scheduled blocks) with the math shown, runnable unattended inside a Flow.
category: Scheduling & Dispatch
tools: [search_tickets, search_members, list_ticket_priorities, update_ticket, add_ticket_note]
---

# Workload-Balancing Assignment

Score every eligible technician's availability with an explicit formula and assign the ticket to the highest scorer — with the scores shown, so the choice is auditable rather than a black box.

## When to use

- "Assign this to whoever is least busy" for a single ticket or a small batch.
- A Flow fires on ticket creation and the desk wants load-balanced auto-assignment.
- A dispatcher wants assignment recommendations they can approve in one glance.

## Steps

1. Build the candidate pool with `search_members`: the technicians on the relevant board/team. Remove the ticket's requester (if a member), inactive members, anyone on a stated exclusion list, and anyone excluded by a client-specific routing rule for this ticket's client.
2. For each candidate, compute an **availability score**:
   `score = base capacity − open tickets (priority-weighted) − scheduled blocks today`
   - Base capacity: the desk's stated per-tech capacity, or a uniform default if none given.
   - Priority weighting: count Critical/High heavier than Low (e.g. Critical ×3, High ×2, Medium/Low ×1) — complexity matters, not just ticket count.
   - Scheduled blocks: today's schedule entries reduce availability even when open-ticket count is low.
3. Rank candidates. If the top two scores are effectively tied, do not pick arbitrarily: in attended mode ask the dispatcher; in unattended mode apply the deterministic tie-break (alphabetical by member id) only if the desk opted into it, otherwise leave unassigned.
4. Assign via `update_ticket` and record the rationale in a plain-text internal note via `add_ticket_note`: the winning score, the runner-up, and the formula inputs (e.g. "Assigned <tech>: score 7 (base 10, 2 open weighted 2, 1 scheduled block). Runner-up <tech>: 5.").
5. In attended mode, output the full score table before assigning and confirm.

## Guardrails

- Show the math. Every assignment carries its score breakdown so a human can sanity-check it later.
- Never assign to the requester, an inactive member, or anyone excluded by a routing rule or pinned exclusion.
- Client-specific routing rules beat the score — if a rule names an owner or team for this client, follow it and say so.
- If ticket searches hit result caps, scores may undercount load — flag it and prefer asking over assigning.
- When in doubt (empty candidate pool, unresolvable tie, missing priority data), do nothing and leave the ticket unassigned rather than guessing.

## Unattended (Flows) variant

- Your entire reply is posted as the internal note — no narration, no questions, plain text only.
- Assign only when there is a single clear top scorer after exclusions. Tie or empty pool → make no assignment and post: "Workload balancer: no unambiguous candidate (reason). Left unassigned for dispatcher."
- Never reassign a ticket that already has an owner; if the ticket is assigned, do nothing and post nothing.
- Never change status, priority, or anything other than the assignee and the note.
