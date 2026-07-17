---
name: Liongard Cross-Client Census
description: Answer "which clients run <system>?" across the whole book of business via Liongard's launchpoint inventory — install-base census for zero-day response, EOL waves, vendor-risk events, and standardization planning.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query]
connectors: [Liongard]
---

# Liongard Cross-Client Census

**When to use:** A zero-day or critical CVE lands and you need "which clients run <vendor product>?"; a vendor breach or supply-chain event needs an exposure list across the book in minutes; an EOL wave needs "who still runs <product/version>?"; or standardization/stack questions ("how many clients are on <firewall brand>?").

## Prompt

```
Produce a cross-client install-base census for TARGET_SYSTEM (optionally version-scoped to AFFECTED_VERSIONS) across the whole book of business, using Liongard's launchpoint inventory. Read-only.

1. Enumerate environments (clients) via liongard_environment, then liongard_launchpoint filtered by TARGET_SYSTEM's systemType across all of them. Match generously — inspector systemType naming varies (brand vs. product line), so check plausible aliases among the ~300 inspector types before declaring absence; a miss understates exposure.
2. State the census's honest shape up front: Liongard shows where an INSPECTOR exists, not where the product exists — the result is the *instrumented* install base. Clients running the product without an inspector are invisible. For high-stakes events, pair the launchpoint census with search_tickets across boards for the product name (per-signal result-cap honesty: "at least N") and docs where available. Separate the output into "confirmed via inspector," "suggested by tickets/docs," and "unknown."
3. For each hit capture the triage columns: client, launchpoint last-run status and dataprint age, and — where the question is version-scoped — liongard_metric against each hit's dataprint for the version/build field (verify field paths against the live dataprint; version fields differ per inspector). A CVE affecting 9.x-before-9.2 turns the census into a ranked patch list.
4. Rank by exposure: affected-version confirmed → version unknown but product confirmed → suggested-only. Failed/stale launchpoints rank UP, not down — unknown version on an internet-facing product is exposure, not comfort.
5. Speed beats polish but never honesty: ship the confirmed tier immediately, keep widening the suggested tier — don't hold the list to perfect it. Version claims come from dataprints, dated; "version unknown" is written, not guessed.
6. Offer to create_ticket per affected client only on confirmation — deduplicate against open tickets via search_tickets first, one plain-text ticket per client with its specific evidence. Never blast the book on an unranked census.
7. Output: the census table (client, evidence tier, version where known, dataprint age), the honest-scope statement, and the count summary "N confirmed, M suggested, coverage unknown for the rest." If the partner doesn't run Liongard, the census is tickets + docs only and materially incomplete — say so.
```
