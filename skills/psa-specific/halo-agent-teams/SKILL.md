---
name: Halo Agent Teams
description: For desks synced to HaloPSA — route tickets by Halo's team/section model: team selects the pool, agent selects the person, and unassigned-within-team is a legitimate state, not an error.
category: PSA-Specific
tools: [search_tickets, list_boards, update_ticket, add_ticket_note, search_members]
---

# Halo Agent Teams

Applies to: **HaloPSA**. Halo routes work through **teams** (service desk, escalations, projects, onsite — surfaced in Thread as boards/queues depending on the tenant's mapping) with **agents** as members; sections/departments group teams above that. Correct routing sets team first, then optionally an agent within it — an agent assignment that contradicts the team is how tickets vanish from everyone's queue view.

## When to use

- Assigning or escalating tickets on a Halo desk ("send this to escalations", "give it to <user>").
- A ticket is assigned to someone who isn't on the ticket's team and nobody can find it.
- Sweeping for unassigned or orphaned tickets per team.

## Steps

1. Re-fetch the ticket with `search_tickets` at full detail; pull the team list with `list_boards` (confirm how this tenant maps Halo teams into Thread — see `skills/psa-specific/psa-field-mapping-doc` if unclear).
2. Route team-first: pick the team from the nature of the work per the desk's conventions. Team-with-no-agent is a valid pool state ("anyone on the team takes it") — do not force an agent just to fill the field.
3. When assigning an agent, verify membership: confirm via `search_members` and desk convention that the person actually works that team. Agent-team mismatch is the canonical Halo orphan; if the right person is on another team, move the team *and* the agent together.
4. For escalation between teams, pair the move with a plain-text handoff note (`add_ticket_note`) stating why and what the receiving team needs (`skills/escalation/escalation-prep` for content). A bare team change is a silent hot potato.
5. Status interplay: team moves via the desk's escalate/reassign action may also set status (see `skills/psa-specific/halo-status-actions`) — perform the pair together, not the team alone.
6. For sweeps: one `search_tickets` per team for unassigned and stale tickets; disclose result caps. Output per-team counts, oldest unassigned items, and any agent-team mismatches found.

## Guardrails

- **Sync lag:** re-fetch full ticket detail before reassigning — the ticket may already be taken, moved, or actioned in Halo.
- Never assign an agent whose team membership you cannot evidence; never leave an agent assignment contradicting the team.
- Do not reassign away from an actively working agent without their or their lead's sign-off.
- Round-robin or load-based assignment logic belongs to `skills/scheduling-and-dispatch/workload-balancing-assignment`; this skill enforces Halo's team mechanics around whatever logic the desk uses.
- Result-cap honesty on sweeps.
- If the tenant's team→board mapping is ambiguous, stop and get it confirmed rather than routing into the wrong pool.

## Cross-references

- Escalation content: `skills/escalation/escalation-prep`
- Assignment logic: `skills/scheduling-and-dispatch/workload-balancing-assignment`, `skills/scheduling-and-dispatch/skill-based-routing`
