---
name: PSA New Board Setup
description: Designing a new board/queue on a PSA-synced desk — a checklist for statuses, ticket types, SLA mapping, and the Thread View + Flow implications, so the board is coherent and syncs cleanly from day one.
category: PSA-Specific
tools: [list_boards, list_ticket_statuses, list_ticket_priorities, list_flows, create_flow]
---

# PSA New Board Setup

Applies to: **any PSA** (ConnectWise Manage, Autotask, HaloPSA) synced to Thread. Standing up a new board/queue (a VIP desk, a project queue, an after-hours board, a new client tier) is a **design** task, not a data task — and it's usually done in the PSA, then mirrored into Thread. Get the status list, types, SLA mapping, and downstream Thread implications right up front, or the board ships with statuses that don't map, Views that don't filter, and Flows that fire on the wrong board. This skill is the checklist form for that design conversation.

## When to use

- "We're adding a new board/queue — help me design it."
- Planning a VIP, project, after-hours, or new-client-tier board before creating it in the PSA.
- Reviewing a proposed board layout for gaps before it goes live.
- A board was created but its statuses/types/SLAs or Thread Views/Flows feel incoherent.

## Steps

Work the checklist, capturing decisions as you go. This produces a design spec a human enacts in the PSA (and, where noted, in Thread).

1. **Purpose & population.** State what the board is *for* in one line and what tickets belong on it (which clients, work types, or trigger). A board without a crisp population rule becomes a dumping ground. Decide whether tickets are routed here manually or by a Flow condition.
2. **Statuses.** Design the status list as a workflow, not a pile: an entry status, the in-progress states, the waiting/hold states (which stop the SLA clock), and the closed-family states. Note per-status side effects the PSA will attach (notifications, SLA pause, closed-family). Remember statuses are **per-board** in CW/Halo — this list is specific to this board. Cross-check the closed-family design against `skills/psa-specific/psa-closed-status-taxonomy`.
3. **Types / subtypes / items.** Decide the type taxonomy for the board and keep it minimal and non-overlapping — reuse the desk's existing taxonomy where it fits rather than inventing parallel values (see `skills/psa-specific/psa-taxonomy-cleanup` and, for CW, `skills/psa-specific/cw-type-subtype-item`). Every added value is future cleanup debt.
4. **SLA mapping.** Map each priority (`list_ticket_priorities`) to the board's response/resolution targets, and confirm which statuses pause the clock (from step 2). State the business hours/calendar the SLA runs against. An SLA with no defined pause states or calendar will misfire.
5. **Agreement & billing implications.** Note how tickets on this board bill — default agreement, whether work is billable, and how it feeds month-end (`skills/psa-specific/psa-billing-cycle-prep`). A board whose billing attributes are undefined creates invoice anomalies later.
6. **Thread View implications.** Specify the Views the desk needs for this board (queue view, waiting-on-customer, breaching-SLA) so the board is actually workable in Thread the day it appears. Note that Views are built in-app; this step defines *which* Views, not the API to create them.
7. **Flow implications.** Determine what should fire on this board and whether Flows can express it. Flows are **ticket-event triggered against conditions** (board, status, priority, type, etc.) — they are **not scheduled** and **cannot** trigger on ticket age / time-in-status / elapsed time. So "route incoming to this board", "set priority on entry", or "run a skill when status enters X" are valid Flow designs; "escalate after N hours idle" is not (that's a manual sweep). Check existing Flows with `list_flows` for overlap before proposing new ones; only `create_flow` after the design is confirmed.
8. **Output** a plain-text board design spec organized by the sections above, with a clear split between **PSA-side actions** (create the board, statuses, types, SLAs — human work in the PSA, the master system) and **Thread-side actions** (Views to build, Flows to create). End with an ordered enactment sequence and what to verify after the board syncs into Thread.

## Guardrails

- **Design output, not silent creation.** Default to producing the spec. Do not create Flows or other Thread objects until the design is confirmed — and never create anything on the PSA side (board/statuses/types/SLAs are PSA work; Thread mirrors them).
- **The PSA is master.** The board is authored in the PSA and syncs to Thread; design accordingly and don't propose Thread as the source of board structure.
- **Don't over-promise Flows.** Only propose Flow behavior that fits real conditions and actions (no scheduled/age-based triggers; a flow's native actions are limited — email/ticket-create/time-log only happen when a flow calls Run Skill / New Super Magic Agent). State the limit rather than designing a Flow that can't exist.
- Validate every referenced status/priority against `list_ticket_statuses` / `list_ticket_priorities` for the actual board once it exists; do not assume values from another board carry over.
- Keep the type taxonomy minimal — flag when a proposed value duplicates an existing one.
- Plain-text spec; no markdown/emojis in anything destined for a PSA note.

## Cross-references

- Status/closed-family design: `skills/psa-specific/psa-closed-status-taxonomy`, `skills/psa-specific/cw-status-workflow-mapping`
- Type taxonomy discipline: `skills/psa-specific/psa-taxonomy-cleanup`, `skills/psa-specific/cw-type-subtype-item`
- Billing feed: `skills/psa-specific/psa-billing-cycle-prep`
- Field mapping doc: `skills/psa-specific/psa-field-mapping-doc`
