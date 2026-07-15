---
name: Zapier Salesforce Case Context
description: Pull account context from Salesforce onto an inbound ticket — SOQL lookups for account standing, owner, open opportunities — and log a case note back when the desk's outcome matters to the account team. Read-mostly discipline. Use for "what does Salesforce say about <client>", "who owns this account", or "log this back to their Salesforce".
category: Connectors
tools: [search_tickets, search_clients, add_ticket_note, "zapier: Salesforce"]
---

# Zapier Salesforce Case Context

When the CRM is Salesforce, the account truth — owner, tier, renewal, open opportunities — lives there. This skill reads that truth onto the ticket with targeted SOQL via `zapier: Salesforce "Find Record" / SOQL query`, and writes back only the narrow thing the account team needs: a note on the right record. Read-mostly is the posture; the desk is a guest in someone else's system of record.

## When to use

- An inbound ticket from a name the desk half-knows: "pull the Salesforce context on <client> before I reply."
- "Who's the account owner — I need to loop them in on this escalation."
- "Log the outage resolution to their Salesforce account so the AE isn't blindsided at renewal."

## Steps

1. Resolve the account precisely: SOQL against Account by domain/exact name from the client record (`search_clients`). Zero or multiple matches → report that and stop the lookup there; never attach another account's context to a ticket.
2. Pull the context that changes desk behavior, nothing more: account owner, type/tier, open opportunities (name, stage, close date), recent cases if the org tracks them, renewal-relevant fields the tenant names. Keep SOQL SELECTs to named fields — no `SELECT *` habits, no crawling unrelated objects.
3. Summarize onto the ticket: `add_ticket_note` (plain text) — the fields found, as of now, with "per Salesforce" provenance. The note serves the tech: "account owned by <user>, renewal <date>, open expansion opp — handle warm."
4. **Write-back (the one allowed write, gated):** when the member asks to log an outcome, compose the note/task text (client-safe, ticket-referenced, outcome-focused), show it with the target record for confirmation, then create it on the Account/Case. That is the write ceiling.
5. If the desk outcome is itself a sales signal, hand the deal-shaped part to the account owner in the write-back text — flag it, don't build pipeline in their CRM.
6. Output ends with: context summary, the ticket note added, and any write-back made.

## Guardrails

- **Member-connected Zapier required** (tenant's Salesforce, and the connected user's permissions bound what's visible — say so when a lookup comes back suspiciously empty). Degrade: proceed without CRM context and state that Salesforce wasn't consulted.
- Read-mostly discipline: lookups and notes only. Never create/edit Accounts, Contacts, Opportunities, stages, or owners — a service agent editing pipeline is how CRM trust dies. Launch Flow / Run Report only on the member's explicit, named request.
- No confident record match → no context; wrong-account context is worse than none.
- CRM data flowing onto tickets is need-to-know: pull fields that change how the ticket is handled, not the account's financial life story.
- Provenance on everything: "per Salesforce as of <date>" — CRM fields go stale and the note must not pretend otherwise.
- Task cost: each Zapier call = 2 tasks — one account query + one targeted follow-up is the expected shape; resist exploratory query chains.
