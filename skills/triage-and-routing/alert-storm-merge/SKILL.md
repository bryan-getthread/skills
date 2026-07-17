---
name: Alert Storm Merge
description: Collapse a burst of identical alert tickets fired within a few hours into the earliest ticket as parent, so a flapping monitor does not flood the queue.
category: Triage & Routing
tools: [search_tickets, merge_ticket, add_ticket_note]
connectors: []
---

# Alert Storm Merge

**When to use:** The queue shows many near-identical alert tickets from one source in a short span, a flow checks whether a new alert belongs to an in-progress storm, or "clean up this alert flood" after a monitor misbehaved overnight.

## Prompt

```
A flapping sensor or repeating monitor can open dozens of identical tickets in a burst.
Find the storm, keep the earliest ticket as the parent, and merge the rest into it with a
count.

1. Read the triggering ticket with search_tickets: client, source tool, device, and the
   exact alert signature (alert name/type + device).

2. Search open tickets with the same signature — same client, same device, same alert
   type — created within the storm window (default: 4 hours). Search per signature per
   board; disclose result caps, since storms often exceed them.

3. Confirm the burst is truly identical: the alert signature must match exactly across all
   candidates. Different devices or alert types are not part of this storm — distinct
   devices alerting with the same symptom may be a real outage; hand that to the
   cross-client/outage path instead of merging.

4. Identify the parent: the earliest-created ticket in the burst that is still open. If
   the earliest is closed, use the earliest still-open ticket; never merge into a closed
   ticket.

5. Merge every later identical ticket into the parent with merge_ticket, in batches,
   re-searching between batches if a cap was hit. Never merge a ticket carrying any human
   reply — pull it out of the batch and flag it.

6. Post one plain-text note on the parent: alert signature, number of tickets merged (the
   exact count and their numbers, or count plus a range if very large), time span of the
   burst, and a suggestion to review the monitor's threshold or flap suppression.

Notes are plain text.

Unattended (Flows): your entire reply is the parent-ticket note, verbatim: "ALERT STORM:
merged <count> tickets into #<parent>. Signature: <signature>. Span: <first>-<last>.
Review monitor thresholds." or "NO STORM: <one line>." Merge only tickets with zero human
replies and an exact signature match; when in doubt about any single ticket, skip that
ticket, not the run. Hard cap per run (default: 25 merges); if more remain, note the
remainder for human review rather than continuing.
```
