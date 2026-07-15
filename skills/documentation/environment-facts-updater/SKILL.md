---
name: Environment Facts Updater
description: When a ticket reveals a client environment fact changed — new server, ISP, VPN, key contact — draft the corresponding documentation update for the client's doc platform.
category: Documentation
tools: [search_tickets, search_itglue, search_hudu, notion-search, notion-fetch, notion-update-page, add_ticket_note]
---

# Environment Facts Updater

Catch the environment changes that surface mid-ticket and turn each into a precise,
ready-to-apply doc update so the documentation keeps matching reality.

## When to use

- A ticket mentions a new server, replaced firewall, changed ISP, new VPN endpoint, or a moved office.
- "Update the docs — <client> switched to a new provider."
- Post-project cleanup: "what did this migration ticket change that our docs should reflect?"
- stale-doc-hygiene flagged a mismatch and the fix is now known.

## Steps

1. Read the ticket thread with `search_tickets` and extract each **confirmed** environment fact change: what changed, old value → new value, effective date, and the evidence line in the thread. A plan or recommendation is not a change — zero-assumption applies.
2. Locate the affected documentation: `search_itglue` / `search_hudu` where enabled, and `notion-search` + `notion-fetch` if the member's docs live in Notion. Note every doc that references the old value.
3. Draft the update per doc, in a paste-ready block:
   - Doc title + location
   - Section/field affected
   - Old value → new value
   - Source: ticket number and date
4. Apply the update only where a writable path exists **and** the requester confirms: Notion pages via `notion-update-page`. IT Glue and Hudu are **search-only** from here — for those, output the paste-ready block and flag it as a manual update for the tech.
5. Post a plain-text internal note on the ticket via `add_ticket_note` (on request) recording which docs were updated or queued for manual update, so the change ticket carries its documentation trail.

## Guardrails

- Only document facts the thread confirms took effect. Never promote "we plan to switch ISPs" into a documented change.
- Confirm before any write: show the exact old → new diff and get a yes before `notion-update-page`. When in doubt, output the draft and do nothing.
- Never write credentials into documentation drafts; if a change involves new credentials, note "credentials updated — store in credential vault" without the values.
- **Degradation:** IT Glue/Hudu are read-only tools here (and only when enabled); Notion requires the member's connection. With no doc platform reachable, output paste-ready drafts only and say which platforms were not checked.
- Keep one fact per update block — bundled edits hide mistakes.
