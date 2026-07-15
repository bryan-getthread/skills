---
name: Monitoring Companion Merge
description: Merge companion tickets that multiple monitoring tools opened for the same underlying event on the same device, folding them into one parent within a time buffer.
category: Triage & Routing
tools: [search_tickets, merge_ticket, add_ticket_note]
---

# Monitoring Companion Merge

When RMM, backup, EDR, and uptime tools each open their own ticket for one real-world event (a server goes down and three tools notice), fold the companions into a single parent so one tech works one incident.

## When to use

- The same device or site generated near-simultaneous tickets from different monitoring tools.
- A flow runs on new alert tickets to check for companions from sibling tools.
- Queue cleanup after an outage left a cluster of same-event alert tickets.

## Steps

1. Read the new alert ticket with `search_tickets`: client, device/asset identifier, event type, and alert timestamp.
2. Search open tickets for the same client referencing the same device (hostname, asset tag, or IP as it appears in alert fields) created within the time buffer (default: 30 minutes either side). Search per identifier, per board if alerts land on multiple boards; disclose any result caps.
3. Confirm the candidates describe the same underlying event: consistent device and a compatible symptom family (offline + heartbeat lost + backup unreachable is one event; offline + low disk is not).
4. Choose the parent: the earliest-created ticket among the companions, unless a human is already actively working a later one — then that one is the parent.
5. Merge each companion into the parent with `merge_ticket`, then post one plain-text note on the parent listing the merged ticket numbers, the tools they came from, and the shared device/event.

## Guardrails

- Same device is mandatory. Same client + similar symptom on different devices is never a companion merge.
- Outside the time buffer → not a companion; leave it.
- Never merge a ticket that contains a human (client or tech) message beyond the alert content — flag it instead.
- If it is unclear whether two alerts describe one event or two, do not merge; note the relationship.
- Do not merge across clients under any circumstances.
- Notes are plain text.

## Unattended (Flows) variant

- Your entire reply is the parent-ticket note, verbatim: `COMPANION MERGE: merged #<a>, #<b> into #<parent>. Device: <device>. Event: <one line>.` or `NO COMPANIONS FOUND for #<n>. Checked window: <buffer>.`
- Merge only when device match is exact and all companions are pure alert tickets with no human replies. When in doubt, do nothing.
- Cap unattended merges at the configured maximum per run (default: 5); beyond that, stop and flag a possible storm for the alert-storm skill.
