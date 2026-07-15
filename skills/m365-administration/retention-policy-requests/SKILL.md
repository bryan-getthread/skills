---
name: Retention Policy Requests
description: Handle requests to change retention or deletion policies in Microsoft Purview — confirm exact scope, warn about legal-hold interaction and irreversible deletion, and gate on documented authorization. Use when a ticket asks to keep mail for N years, auto-delete old items, or change what's retained.
category: M365 Administration
tools: [search_tickets, search_contacts, search_clients, search_knowledge_base, search_itglue, add_ticket_note, send_approval, log_time_entry, web_search]
---

# Retention Policy Requests

Turns a vague "keep email for seven years" request into a precisely scoped,
authorized, reversible-where-possible retention change — with the deletion
consequences and legal-hold interactions stated before anything ships.

## When to use

- "Retain all mail for N years for compliance."
- "Auto-delete anything older than X in <location>."
- "Why can't this user permanently delete emails?" (existing policy discovery).
- NOT for placing a legal hold — that is litigation-hold; this skill manages
  the general retention machinery around it.

## Steps

1. Decompose the ask into Purview terms and confirm each with the requester:
   - Retain, delete, or retain-then-delete?
   - Duration, and from when (created vs last modified)?
   - Scope: which workloads (Exchange, SharePoint, OneDrive, Teams) and which
     users/sites — whole org or specific locations? Never assume org-wide.
   - Adaptive or static scope, if the tenant uses adaptive scopes.
2. Discover current state first: have the tech list existing retention
   policies and labels touching the requested scope, plus any litigation or
   eDiscovery holds on the affected mailboxes. Paste into the ticket. A new
   policy lands on top of this, and the principles of retention decide the
   winner (retention wins over deletion; longest retention wins; explicit
   scope beats broad).
3. State the legal-hold interaction in the ticket before approval, in plain
   words: a litigation hold or eDiscovery hold overrides any deletion policy —
   held items are preserved regardless. Conversely, adding a deletion policy
   never "cleans up" held data, and removing a retention policy from mailboxes
   under hold does not release the data. If the client believes deletion will
   apply to held mailboxes, correct that expectation in writing.
4. Warn on the irreversible parts:
   - A deletion policy, once it starts processing, destroys data past the
     window — there is no undo for what's already purged.
   - Preservation Lock, if requested, cannot be shortened or removed once
     applied. Refuse to include it without explicit written client direction
     acknowledging permanence.
5. Approval gate: retention changes require the client's documented
   compliance/legal authority — not just "someone in IT" (send_approval).
   For anything with a delete action, the approval text must restate the
   scope and the destruction consequence.
6. Prepare the change for the tech (Purview portal steps; PowerShell like
   `New-RetentionCompliancePolicy` / `New-RetentionComplianceRule` — verify
   against current module versions). Prefer starting scoped to a pilot
   location before org-wide where the client allows it.
7. Verify via evidence after propagation (policies can take up to a week to
   fully apply): policy shows distributed/On, and a spot-check item behaves as
   expected. Set that expectation in the ticket.
8. Post a plain-text note: policy name, exact scope and locations, retain vs
   delete behavior and duration, interaction with existing holds found in
   step 2, approver, application date, and rollback (disable policy <name>;
   note what deletion may already have processed and cannot be rolled back).
   Log time.

## Guardrails

- No deletion policy ships without a named client authority approving the
  destruction consequence in writing.
- Never claim a deletion policy will remove data under legal hold; the hold
  wins, always say so.
- Preservation Lock is a one-way door — require explicit acknowledgment of
  permanence or leave it out.
- Scope creep check: "all mail" requests get restated as an explicit location
  list before approval.
- When existing policies conflict with the request, present the
  principles-of-retention outcome rather than promising the requested
  behavior.
