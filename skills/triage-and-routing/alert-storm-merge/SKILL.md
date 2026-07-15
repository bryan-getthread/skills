---
name: Alert Storm Merge
description: Collapse a burst of identical alert tickets fired within a few hours into the earliest ticket as parent, so a flapping monitor does not flood the queue.
category: Triage & Routing
tools: [search_tickets, merge_ticket, add_ticket_note]
---

# Alert Storm Merge

A flapping sensor or repeating monitor can open dozens of identical tickets in a burst. This skill finds the storm, keeps the earliest ticket as the parent, and merges the rest into it with a count.

## When to use

- The queue shows many near-identical alert tickets from one source in a short span.
- A flow runs on new alert tickets to check whether they belong to an in-progress storm.
- "Clean up this alert flood" after a monitor misbehaved overnight.

## Steps

1. Read the triggering ticket with `search_tickets`: client, source tool, device, and the exact alert signature (alert name/type + device).
2. Search open tickets with the same signature — same client, same device, same alert type — created within the storm window (default: 4 hours). Search per signature per board; disclose result caps, since storms often exceed them.
3. Confirm the burst is truly identical: the alert signature must match exactly across all candidates. Different devices or different alert types are not part of this storm.
4. Identify the parent: the earliest-created ticket in the burst that is still open.
5. Merge every later identical ticket into the parent with `merge_ticket`, in batches, re-searching between batches if a cap was hit.
6. Post one plain-text note on the parent: alert signature, number of tickets merged, time span of the burst, and a suggestion to review the monitor's threshold or flap suppression.

## Guardrails

- Exact signature match only (client + device + alert type). "Looks similar" is not a storm member.
- Never merge a ticket carrying any human reply — pull it out of the batch and flag it.
- If the earliest ticket is closed, use the earliest still-open ticket as parent; never merge into a closed ticket.
- Distinct devices alerting with the same symptom is not a storm — that may be a real outage; hand it to the cross-client/outage path instead of merging.
- Keep an auditable count: the parent note must state exactly how many tickets were merged and their numbers (or the count plus a range if very large).
- Notes are plain text.

## Unattended (Flows) variant

- Your entire reply is the parent-ticket note, verbatim: `ALERT STORM: merged <count> tickets into #<parent>. Signature: <signature>. Span: <first>-<last>. Review monitor thresholds.` or `NO STORM: <one line>.`
- Merge only tickets with zero human replies and an exact signature match. When in doubt about any single ticket, skip that ticket, not the run.
- Hard cap per run (default: 25 merges); if more remain, note the remainder for human review rather than continuing.
