---
name: Dispatch & Workload
description: Balance the queue by showing open work per technician and assigning new tickets to the lightest load.
category: Triage & Routing
tools: [search_tickets, search_members, update_ticket, list_boards, schedule_ticket]
---

# Dispatch & Workload

**When to use:** A dispatcher needs to distribute new or unassigned tickets without overloading anyone, or wants a morning dispatch view.

**What you get:** A per-technician breakdown of open tickets and a recommended assignment for each unassigned ticket.

## Steps

1. Pull unassigned and open tickets across the support boards.
2. Count open work per technician and identify who has capacity.
3. For each unassigned ticket, recommend an owner based on load and, where known, skill fit.
4. Produce a single morning briefing: unassigned queue, workload by tech, and suggested assignments.

## Guardrails

- Recommend assignments. Confirm before committing them unless the team has opted into auto-assign.
- Respect scheduled and in-progress work when judging capacity.

## Consolidates

Load-balanced dispatch, technician workload summary, and daily/dispatcher morning briefing skills.
