---
name: Stale Doc Hygiene
description: Find documentation untouched for 180+ days, test it against current ticket reality, and produce a verification task list of docs to confirm, update, or retire.
category: Documentation
tools: [search_itglue, search_hudu, notion-search, notion-fetch, search_knowledge_base, search_tickets]
---

# Stale Doc Hygiene

Age-audit the documentation platform against what tickets say is actually true today, and
turn the mismatches into a concrete verification task list. Nothing is edited or deleted.

## When to use

- "Which of our docs are stale?" / "audit <client>'s documentation."
- A doc was just found to be wrong mid-ticket and the team suspects there are more.
- Scheduled documentation-hygiene sweeps (quarterly, per client onboarding anniversary).
- Before an audit or client handover: "is this client's documentation trustworthy?"

## Steps

1. Identify the doc surface for this tenant: `search_itglue` and/or `search_hudu` where enabled, `notion-search` + `notion-fetch` if the member has connected Notion, and the Thread KB via `search_knowledge_base`. Scope to one <client> or category when asked — a full-estate sweep should be run per client to stay within result caps.
2. Collect candidate docs not modified in **180+ days** (use last-updated metadata where the platform returns it; where it does not, mark age as "unknown" rather than guessing).
3. Reality-check each stale doc against tickets: `search_tickets` for recent activity touching the same system/procedure. Flag mismatches such as:
   - Tickets reference a server, ISP, VPN, or vendor the doc does not mention (or vice versa).
   - Recent resolutions contradict the doc's procedure.
   - The doc covers a system with zero ticket activity in 12+ months (possible retirement).
4. Build the **verification task list**, one row per doc: doc title + location, last-touched age, what tickets suggest changed (with ticket numbers), and a recommended action — **Confirm** (probably fine, quick check), **Update** (specific mismatch found), or **Retire** (system appears gone). Suggest an owner only if the requester names the team.
5. Output the list ranked: security-relevant docs first, then client-facing procedures, then internal reference. Offer to draft updates for the top items via environment-facts-updater or kb-article-draft.

## Guardrails

- Never edit, move, or delete documentation — the output is a task list for humans.
- A ticket mismatch is evidence a doc *may* be stale, not proof — every item is phrased as "verify", and the cited tickets are real ticket numbers.
- **Result-cap honesty:** doc platforms and ticket search both cap; state when the sweep is partial and which client/category was actually covered.
- Degradation: if no doc platform is enabled and Notion is not connected, run against the Thread KB only and say so. Notion tools require the member to have connected Notion; results are scoped to that member's access.
- Do not surface credential entries in the report — reference them by title only.
