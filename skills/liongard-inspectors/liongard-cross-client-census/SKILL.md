---
name: Liongard Cross-Client Census
description: Answer "which clients run <system>?" across the whole book of business via Liongard's launchpoint inventory — install-base census for zero-day response, EOL waves, vendor-risk events, and standardization planning.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, search_tickets, create_ticket, add_ticket_note]
---

# Liongard Cross-Client Census

The killer cross-client move: because every Liongard inspector is a launchpoint tied to an environment, the launchpoint inventory IS an install-base census. When a zero-day drops, a vendor gets breached, or a product goes end-of-life, "which of our clients run this?" stops being a week of tribal-knowledge polling and becomes one read — with version detail where the dataprints carry it.

## When to use

- A zero-day or critical CVE lands: "which clients run <vendor product>?" (feeds security/zero-day-emergency-response step one)
- A vendor breach or supply-chain event: exposure list across the book in minutes
- An EOL wave: "who still runs <product/version>?" for planned migrations (pairs with communication/eol-product-notice)
- Standardization/stack questions: "how many clients are on <firewall brand>?" / M&A-style stack inventory of a book segment

## Steps

1. Enumerate the environments (clients) via liongard_environment, then liongard_launchpoint filtered by the target systemType across all environments (per-launchpoint trust rules per skills/liongard-inspectors/liongard-access-pattern/SKILL.md). Match generously: inspector systemType naming varies (brand vs. product line), so check plausible aliases among the ~300 inspector types before declaring absence — a miss here understates exposure.
2. State the census's honest shape immediately: Liongard shows where an INSPECTOR exists, not where the product exists. The result is "instrumented install base." Clients running the product without an inspector are invisible — so pair the launchpoint census with a cross-check for high-stakes events: search_tickets across boards for the product name (per-signal, result-cap honesty: "at least N") and docs where available. The output separates "confirmed via inspector," "suggested by tickets/docs," and "unknown."
3. For each hit, capture the triage columns: client, launchpoint last-run status and dataprint age, and — where the question is version-scoped — liongard_metric against each hit's dataprint for the version/build field (verify field paths against the live dataprint; version fields differ per inspector). A CVE affecting 9.x-before-9.2 turns the census into a ranked patch list.
4. Rank the output by exposure: affected-version confirmed → version unknown but product confirmed → suggested-only. Failed/stale launchpoints rank up, not down — unknown version on an internet-facing product is exposure, not comfort.
5. Hand off by scenario: zero-day → deliver the ranked list into security/zero-day-emergency-response (it owns comms and remediation tracking); EOL wave → devices-and-infrastructure/warranty-eol-report and client comms via communication/eol-product-notice; vendor breach → the matching vendor-runbooks skill for per-client response. Offer to create_ticket per affected client (deduplicate against open tickets first) — only on confirmation, plain-text, one per client with its specific evidence.
6. Output: the census table (client, evidence tier, version where known, dataprint age), the honest-scope statement, and the count summary "N confirmed, M suggested, coverage unknown for the rest."

## Guardrails

- Never present the launchpoint census as the true install base — the absence of an inspector is absence of evidence, and the output must say so every time.
- Version claims come from dataprints, dated; "version unknown" is written, not guessed.
- In a zero-day, speed beats polish but not honesty: ship the confirmed tier immediately, keep widening the suggested tier — do not hold the list to perfect it.
- Ticket creation is confirmed, deduplicated, and per-client; never blast the book with tickets on an unranked census.
- Result-cap honesty on every ticket cross-check; split searches per signal per board.
- Degradation: partner doesn't run Liongard → the census is tickets + docs only and materially incomplete; say so and recommend the census capability as its own value line.
