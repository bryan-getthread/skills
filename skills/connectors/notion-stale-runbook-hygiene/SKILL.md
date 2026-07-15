---
name: Notion Stale Runbook Hygiene
description: Find runbooks and SOPs not edited in 180+ days and comment @owner asking for verification — a recurring documentation-rot sweep. Use when asked to "find stale runbooks", "audit our Notion docs for outdated pages", or "nudge owners to verify old SOPs".
category: Connectors
tools: [notion-search, notion-query-data-sources, notion-fetch, notion-create-comment, notion-get-comments, notion-get-users]
---

# Notion Stale Runbook Hygiene

Documentation rots silently. This sweep finds runbook pages past their freshness window, checks they haven't already been nudged, and posts a polite verification request to the page owner — turning "someday" doc maintenance into a queue.

## When to use

- "Which of our runbooks haven't been touched in six months?"
- Scheduled quarterly documentation-hygiene pass.
- "Ping the owners of anything stale in the runbooks teamspace."

## Steps

1. Confirm scope (the runbooks teamspace/database) and threshold (default: last edited 180+ days ago).
2. Query for stale pages: `notion-query-data-sources` on the runbooks database filtered by last-edited (or a Last verified property if the database has one — prefer it, since a page can be edited without being verified). Fall back to `notion-search` + `notion-fetch` metadata when no database backs the pages.
3. For each stale page, resolve the owner: the Owner property if present, else last editor (`notion-get-users` to resolve identities). No resolvable owner → route to the member instead of guessing.
4. **Dedupe check:** `notion-get-comments` on each page; if a verification request from this process exists within the last 30 days, skip — no repeat-nudging.
5. Present the stale list (page, owner, days stale) and get confirmation for the comment batch.
6. `notion-create-comment` on each confirmed page, @mentioning the owner: a short, neutral request to verify accuracy, update the Last verified date, or mark the page for archival if the system it covers is gone.
7. Report: commented pages, skipped (recently nudged, no owner), and a suggested archive-candidate list (stale + owner departed + system unknown).

## Guardrails

- **Member-authenticated connector:** requires the member's Notion connection and only sees pages that member can access — say so in the report ("scoped to your access"). Degrade by outputting the stale list for manual follow-up.
- Comment, never edit: this skill must not modify or archive pages itself — it requests verification; owners act.
- The 30-day comment dedupe is mandatory; hygiene bots that nag get muted, then ignored.
- Cap a single run's comments (default 20) and say when the stale list was truncated.
- Neutral tone in comments — the owner is a colleague, not a delinquent; no shame metrics in the @mention.
