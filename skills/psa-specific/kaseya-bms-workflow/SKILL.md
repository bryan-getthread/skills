---
name: Kaseya BMS Workflow
description: For desks synced to Kaseya BMS — navigate the BMS status/queue/location model, respect the service-desk vs projects module split, and audit Thread↔BMS drift the same way as the CW/Halo sync audits.
category: PSA-Specific
tools: [search_tickets, list_boards, list_ticket_statuses, update_ticket, add_ticket_note, search_clients]
---

# Kaseya BMS Workflow

Applies to: **Kaseya BMS (Vorex lineage)**. BMS organizes service tickets by **queue** (the routing unit Thread mirrors as a board), **status** (a global list, not per-queue like ConnectWise), **ticket type**, and **location** (client site — many BMS desks are strict about location because it drives dispatch and contract selection). Service Desk and Projects are separate BMS modules: project tasks live outside the service-ticket workflow and may not sync into Thread at all. This skill keeps status/queue changes inside the BMS model and reconciles drift.

## When to use

- "Move this ticket to <status/queue>" on a BMS-synced desk and you need the safe equivalent.
- A ticket shows different statuses or queues in Thread and in BMS.
- Someone asks why a "ticket" they see in BMS isn't visible in Thread (often: it's a project task, not a service ticket).
- Periodic drift audit on a BMS-synced desk.

## Steps

1. Re-fetch the ticket with `search_tickets` at full detail — never act on a status or queue seen earlier in the conversation. BMS→Thread sync can lag by minutes.
2. Pull the live status list with `list_ticket_statuses` and the queue/board list with `list_boards`. BMS statuses are tenant-configured from a global list: do not assume the common defaults (New, Assigned, In Progress, Waiting for Customer, Completed) exist on this tenant — verify.
3. For status moves: map the request onto a status that actually exists, and classify it before writing — is it completed-family (closes in BMS), does it stop the SLA clock (waiting-type), does a BMS workflow/notification trigger on it? State each side effect in your proposal.
4. For queue moves: confirm the target queue exists via `list_boards` and warn that queue changes in BMS can re-trigger routing/assignment workflows and change which SLA applies. Confirm before moving.
5. **Service desk vs projects:** if a referenced item cannot be found with `search_tickets`, consider that it may be a BMS project task. Project tasks follow the Projects module's own task statuses and generally do not appear in Thread. Say so explicitly rather than reporting "ticket does not exist," and route project work to a human with BMS access.
6. **Location awareness:** when a client has multiple locations/sites, verify the ticket's location against the contact and reported site (`search_clients` for the client record; the desk's site conventions). A wrong location can bill against the wrong contract in BMS — flag mismatches, don't silently fix them.
7. **Sync-audit variant** (on request, per queue): split searches per signal — (a) tickets completed-family in BMS but open in Thread, (b) open tickets whose queue in Thread doesn't match the desk's documented map, (c) tickets stuck in a waiting status past the desk's threshold. Present counts and samples; if any search may have hit a result cap, say the count is "at least N," not exact. Reconcile per `skills/psa-specific/psa-is-master-reconciliation` — one ticket at a time, propose before applying.
8. Output: current state in each system, the proposed change, side effects, and the exact `update_ticket` call — apply only after confirmation, then record with a plain-text `add_ticket_note`.

## Guardrails

- **Sync lag:** always re-fetch full ticket detail immediately before trusting or changing status/queue. A stale list result is not evidence of divergence.
- **PSA is always master.** When Thread and BMS disagree, Thread moves to match BMS — never the reverse — unless the desk documents an exception.
- Never set a status or queue that `list_ticket_statuses` / `list_boards` did not return. BMS defaults vary by tenant; do not invent values.
- Never close a ticket as a side effect of a status move — completed-family transitions are deliberate acts with their own QA gate (`skills/qa-and-closure/closure-note-completeness`).
- Do not touch project tasks. If it lives in the BMS Projects module, report and hand off.
- Notes that sync to BMS must be plain text — no markdown, no emojis.
- Degradation: if this tenant's BMS sync does not carry queues or locations into Thread, run in advisory mode from the desk's documented map and state that limitation.

## Cross-references

- Generic reconciliation: `skills/psa-specific/psa-is-master-reconciliation`
- Pattern siblings: `skills/psa-specific/cw-status-workflow-mapping`, `skills/psa-specific/cw-sync-lag-audit`, `skills/psa-specific/halo-sync-audit`
- Note visibility: `skills/psa-specific/psa-note-visibility-rules`
