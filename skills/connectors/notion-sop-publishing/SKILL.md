---
name: Notion SOP Publishing
description: Turn a resolved ticket into an SOP page in the team's Notion runbooks teamspace, tagged by client, product, and category — with a link posted back to the ticket. Use when asked to "write this up as an SOP", "publish this fix to Notion", or "document this resolution in the runbooks".
category: Connectors
tools: [search_tickets, notion-search, notion-fetch, notion-create-pages, notion-update-page, add_ticket_note]
---

# Notion SOP Publishing

Capture a hard-won resolution while it's fresh: extract the problem, environment, and fix from the ticket thread, publish it as a structured SOP in the runbooks teamspace, and link it back to the ticket so the next tech finds it.

## When to use

- "That was painful — write it up as an SOP so we never rediscover it."
- "Publish the resolution from ticket <number> to our Notion runbooks."
- A closure-QA flow flags a novel resolution worth documenting.

## Steps

1. Read the full ticket thread (`search_tickets`, include messages and internal notes) and extract: symptom as the user reported it, environment facts (<application>, OS/version, relevant config), diagnostic path, the fix that worked — and what was tried that did NOT work.
2. `notion-search` the runbooks teamspace for an existing SOP on the same issue. If one exists, propose updating it (`notion-update-page`) with the new findings instead of creating a duplicate.
3. Draft the SOP: title in the desk's controlled vocabulary ("<Product> — <symptom>: <fix summary>"), sections for Symptoms, Environment, Resolution steps (numbered, imperative), What didn't work, and Source ticket reference. Sanitize as you draft — see guardrails.
4. Show the draft and the target location (the runbooks teamspace/database) for confirmation.
5. `notion-create-pages` into the runbooks location, setting the client, product, and category tags/properties the database defines.
6. `add_ticket_note` (internal, plain text) on the source ticket: "SOP published: <page title> — <link>".

## Guardrails

- **Notion is member-authenticated: the member must have connected Notion, and pages land with their access.** If Notion tools are absent, tell the member to connect Notion in their settings; degrade by outputting the finished SOP text for manual paste.
- Sanitization is mandatory before publish: no passwords, license keys, or shared secrets in the SOP — reference the credential store's entry name instead ("credentials in the documentation vault under <entry>"). End-user personal details are replaced with <user>.
- Only document what the ticket evidence shows worked; never pad the SOP with plausible extra steps. No invented links or KB references.
- Search-before-create is required — duplicate SOPs are how runbook trust dies.
- Confirm the target teamspace on first use and reuse it thereafter; never scatter SOPs into private pages.
