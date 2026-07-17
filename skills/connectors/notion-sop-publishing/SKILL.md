---
name: Notion SOP Publishing
description: Turn a resolved ticket into an SOP page in the team's Notion runbooks teamspace, tagged by client, product, and category — with a link posted back to the ticket. Use when asked to "write this up as an SOP", "publish this fix to Notion", or "document this resolution in the runbooks".
category: Connectors
tools: [search_tickets, add_ticket_note]
connectors: [Notion]
---

# Notion SOP Publishing

**When to use:** "That was painful — write it up as an SOP so we never rediscover it," or "publish the resolution from ticket <number> to our Notion runbooks."

## Prompt

```
Publish a resolved ticket as a structured SOP in the team's Notion runbooks teamspace.

This needs the member's connected Notion (notion-search, notion-fetch, notion-create-pages,
notion-update-page). If Notion isn't connected, say so, tell them to connect Notion in
settings, and degrade by outputting the finished SOP text for manual paste — do not stop.

1. Read the full ticket thread with search_tickets (include messages and internal notes) and
   extract: symptom as the user reported it, environment (application, OS/version, relevant
   config), the diagnostic path, the fix that worked, and what was tried that did NOT work.
2. notion-search the runbooks teamspace for an existing SOP on the same issue. If one exists,
   propose updating it with notion-update-page rather than creating a duplicate —
   search-before-create is mandatory; duplicate SOPs are how runbook trust dies.
3. Draft the SOP: title as "<Product> — <symptom>: <fix summary>"; sections for Symptoms,
   Environment, Resolution steps (numbered, imperative), What didn't work, Source ticket ref.
   Sanitize as you draft: no passwords, license keys, or shared secrets — reference the
   credential vault entry name instead ("credentials in the documentation vault under
   <entry>"); replace end-user personal details with <user>. Document only what the ticket
   evidence shows worked — never pad with plausible extra steps or invented KB links.
4. Show me the draft and the target teamspace/database, and wait for confirmation before writing.
5. On confirmation: notion-create-pages into the runbooks location, setting the client,
   product, and category tags/properties the database defines. Confirm the target teamspace on
   first use and reuse it thereafter — never scatter SOPs into private pages.
6. add_ticket_note (internal, plain text): "SOP published: <page title> — <link>".
```
