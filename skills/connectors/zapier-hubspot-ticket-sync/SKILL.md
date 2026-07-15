---
name: Zapier HubSpot Ticket Sync
description: Mirror service-desk outcomes into HubSpot — find-or-create the contact/company, log the engagement, and raise an expansion-signal deal when a ticket reveals a sales opportunity. Never creates duplicate CRM records. Use for "log this in HubSpot", "get this expansion signal to sales", or CRM-visibility passes over resolved work.
category: Connectors
tools: [search_tickets, search_clients, search_contacts, add_ticket_note, "zapier: HubSpot"]
---

# Zapier HubSpot Ticket Sync

The desk sees expansion signals and relationship moments sales never hears about. This skill puts the right slice of ticket reality into HubSpot — an engagement on the right company record, a deal when a ticket exposes real buying intent — with a find-first discipline, because a CRM full of duplicate companies is worse than no sync at all.

## When to use

- "Log the outcome of ticket <number> against <client> in HubSpot so the account team sees it."
- A ticket surfaces buying intent ("we're hiring 15 people", "this server is end-of-life") — raise it as a deal-shaped signal.
- A periodic pass syncing notable resolved tickets into CRM activity.

## Steps

1. **Find before create, always:** resolve the company via `zapier: HubSpot "Find Company"` (domain first — it dedupes better than names), and the person via "Find Contact" (email). Exactly-one match → use it. Zero matches → creating is a member decision; show what will be created first. Multiple matches → stop and ask; picking one at random pollutes someone's pipeline.
2. Log the engagement: create a note/engagement on the company (and contact where relevant) with the ticket reference, a client-safe outcome summary, and the date. Write what sales needs — outcome and signal — not the ticket's full technical transcript.
3. **Expansion-signal deals (gated):**
   - Qualify: the signal must be explicit in the ticket (stated need, growth event, EOL asset), not inferred mood.
   - Compose the deal: name "<client> — <signal>", pipeline/stage per the tenant's convention (ask if unknown — never invent a stage), source note referencing the ticket, and **no invented amount** — value blank or member-provided.
   - Dedupe: search existing open deals on the company for the same signal; found → add the new evidence to it rather than a twin deal.
   - Show the composed deal for confirmation, then create and associate it to the company/contact.
4. Close the loop on the ticket: `add_ticket_note` (plain text) — what was logged/created in HubSpot and where, so the desk knows sales has it.
5. Output ends with: records touched (found vs created), engagement logged, deal raised or matched, and anything skipped for ambiguity.

## Guardrails

- **Member-connected Zapier required** (tenant's HubSpot). Degrade: output the formatted engagement/deal text for manual CRM entry and still note the signal on the ticket.
- Never create a duplicate company, contact, or deal — find-first is mandatory, and "no confident match" resolves to asking, not creating.
- CRM entries are read by sales and sometimes surfaced to clients: client-safe wording, no internal blame, no credentials, no end-user personal data beyond business contact fields.
- Deals carry only evidenced signals; inflating a grumble into a pipeline entry burns the desk's credibility with sales.
- This skill writes activity and signals; it never edits lifecycle stages, owners, or existing deal amounts — those belong to the account team.
- Task cost: each Zapier call = 2 tasks — find company + find contact + one write is the expected shape per ticket; batch periodic passes.
