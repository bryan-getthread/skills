---
name: Out-of-Scope Billing Flag
description: When a ticket looks like work outside the client's agreement (projects, new installs, non-covered users or sites) and it should be flagged before hours pile up — with a quote path instead of silent free work.
category: Finance & Billing
tools: [search_tickets, search_clients, add_ticket_note, update_ticket]
connectors: []
---

# Out-of-Scope Billing Flag

**When to use:** "Is this ticket covered under <client>'s agreement or should it be quoted?" / "scan my open tickets for project-sized work hiding on the support board" / "flag out-of-scope work at <client> before we eat more hours."

## Prompt

```
Spot in-flight work that is probably outside the client's agreement, flag it internally before
significant hours accumulate, and route it toward a quote/approval conversation instead of
unbilled delivery.

1. Read the ticket(s) in full with search_tickets (thread + time entries). For a board/client
   sweep, search per client and note result caps.

2. Look for out-of-scope signals: new-project language (install, migrate, deploy, "set up the
   new office"), quantities (multiple machines/users at once), non-covered entities (a site,
   subsidiary, or personal device the requester says isn't on the agreement), and hours
   already logged well past typical incident size.

3. Check the coverage question honestly: Thread does not hold the agreement. If the requester
   provided scope terms (covered services, included users/sites, project threshold), test the
   signals against them and cite the specific term. If not, the output can only say "likely
   out of scope — verify against the agreement", never "not covered".

4. Quantify the exposure so far: hours already logged on the ticket, and a rough
   remaining-effort read from the thread.

5. Flag it: post a plain-text internal note via add_ticket_note — signals found, hours logged
   to date, the agreement term in question (or "agreement not reviewed"), and the recommended
   path: pause non-urgent work, confirm scope with the account owner, quote if out of scope.
   With the requester's confirmation, update_ticket to the desk's scope-review status or board
   if one exists.

6. Output to the requester: the flag summary plus a suggested quote outline (what would be
   quoted, T&M vs fixed suggestion) they can hand to sales.

Guardrails: never state coverage as fact without citing the agreement evidence — "likely out
of scope" is the strongest claim allowed when the agreement hasn't been reviewed. Never tell
the client work is billable, paused, or requires a quote — that conversation belongs to a
human; all flags are internal notes. Do not stop or deprioritize urgent work (outages,
security) over a scope question; flag it and keep the client running — scope is settled after
stabilization. Confidence gate on status/board changes: only move the ticket when the
requester confirms; when running as a sweep, notes only. Time entries are the exposure
evidence — report logged hours as logged, no estimates dressed as actuals.

Running unattended (Flows): trigger on a new ticket or time-entry threshold on a
managed-services board. Your entire reply is posted verbatim as an internal note — no
narration, no questions, plain text only. Post a flag note ONLY when at least two independent
out-of-scope signals are present AND logged hours exceed the flow's configured threshold; one
weak signal → output nothing. Never change status, board, or assignee in unattended mode.
Never write anything client-visible. When in doubt, do nothing.
```
