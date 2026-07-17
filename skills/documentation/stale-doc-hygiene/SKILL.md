---
name: Stale Doc Hygiene
description: Find documentation untouched for 180+ days, test it against current ticket reality, and produce a verification task list of docs to confirm, update, or retire.
category: Documentation
tools: [search_itglue, search_hudu, notion-search, notion-fetch, search_knowledge_base, search_tickets]
connectors: [IT Glue, Hudu, Notion]
---

# Stale Doc Hygiene

**When to use:** "Which of our docs are stale?" / "audit <client>'s documentation" — a doc was just found wrong mid-ticket, a quarterly hygiene sweep, or before an audit or client handover.

## Prompt

```
Age-audit the documentation platform against what tickets say is actually true today,
and turn the mismatches into a concrete verification task list. Never edit, move, or
delete documentation — the output is a task list for humans.

1. Identify the doc surface for this tenant: search_itglue and/or search_hudu where
   enabled, notion-search + notion-fetch if the member connected Notion, and the
   Thread KB via search_knowledge_base. Scope to one <client> or category when asked
   — run a full estate per client to stay within result caps.

2. Collect candidate docs not modified in 180+ DAYS (use last-updated metadata where
   the platform returns it; where it does not, mark age as "unknown" rather than
   guessing).

3. Reality-check each stale doc against tickets: search_tickets for recent activity
   touching the same system/procedure. Flag mismatches such as: tickets reference a
   server/ISP/VPN/vendor the doc doesn't mention (or vice versa); recent resolutions
   contradict the doc's procedure; the doc covers a system with zero ticket activity
   in 12+ months (possible retirement). A ticket mismatch is evidence a doc MAY be
   stale, not proof — phrase every item as "verify", with real ticket numbers.

4. Build the VERIFICATION TASK LIST, one row per doc: doc title + location, last-
   touched age, what tickets suggest changed (with ticket numbers), and a recommended
   action — Confirm (probably fine, quick check), Update (specific mismatch found), or
   Retire (system appears gone). Suggest an owner only if the requester names the team.

5. Output the list ranked: security-relevant docs first, then client-facing
   procedures, then internal reference. Do not surface credential entries in the
   report — reference them by title only. RESULT-CAP HONESTY: doc platforms and ticket
   search both cap; state when the sweep is partial and which client/category was
   actually covered. If no doc platform is enabled and Notion isn't connected, run
   against the Thread KB only and say so.

6. Offer to draft updates for the top items via environment-facts-updater or
   kb-article-draft.
```
