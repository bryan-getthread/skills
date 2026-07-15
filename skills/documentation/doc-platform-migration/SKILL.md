---
name: Doc Platform Migration
description: Plan and track a documentation-platform move (into IT Glue, Hudu, or another system) — full inventory, priority tiers, verification sampling, and cutover discipline so nothing important is lost and the old platform dies on a date.
category: Documentation
tools: [search_itglue, search_hudu, search_knowledge_base, notion-search, notion-fetch, search_tickets, add_ticket_note, create_ticket]
---

# Doc Platform Migration

Turns "we're moving to <platform>" into a migration that ends: an inventory of what exists, tiers that decide what moves first (and what doesn't move at all), sampling that proves the moved content survived intact, and a cutover date after which the old platform is read-only and then gone.

## When to use

- "We're migrating our documentation into IT Glue / Hudu — build the plan."
- Consolidating after an acquisition: two doc platforms need to become one.
- Mid-migration drift: "we started moving docs months ago and it stalled — where are we?"

## Steps

1. **Inventory the source.** Enumerate what exists on the source platform(s) — search_itglue / search_hudu / search_knowledge_base / notion-search as applicable — by client and by type (credentials, configurations, runbooks/SOPs, diagrams, contacts, vendor info, free-text notes). Where search caps prevent full enumeration, count per client/category and mark the inventory as sampled, not exhaustive. Include the awkward tail: attachments, embedded images, and cross-links, which migrate worst.
2. **Assign priority tiers** — every inventoried item gets exactly one:
   - **Tier 1 — move first, verify every item:** credentials (see documentation/credential-storage-audit for how these move), active-client core docs (network, backup/recovery, critical runbooks), anything referenced by a ticket in the last 90 days (search_tickets to test this).
   - **Tier 2 — move in bulk, verify by sample:** active-client reference material without recent ticket traffic.
   - **Tier 3 — do not migrate:** docs for departed clients and retired systems, superseded versions, empty stubs. Tier 3 gets archived/exported once for the record, not moved — a migration is the one chance to not carry the garbage forward. Stale candidates can be pre-identified via documentation/stale-doc-hygiene.
3. **Define verification sampling** in the plan: Tier 1 = 100% human verification (opens, reads, works); Tier 2 = a fixed sample per client per type (e.g. every 10th item, minimum 3) checked for content integrity, attachments present, images render, links repointed. Any Tier 2 sample failure widens verification for that client/type to 100%. Failed items are logged with source + destination references.
4. **Set cutover discipline** as dated commitments the requester confirms:
   - A **freeze date**: from this date, new/edited docs go only to the destination — dual-writing is how migrations never end.
   - Source goes **read-only** at freeze; a redirect note at the source's front door tells techs where to write.
   - A **decommission date** after a defined read-only grace period, contingent on Tier 1 at 100% verified and Tier 2 sampling passed.
   - An owner per client-batch, so "the migration" is never everyone's job and no one's.
5. Output the plan: inventory summary (with sampled/exhaustive marking), tier assignments, verification protocol, cutover dates, per-batch owners. If asked, create tracking tickets per client-batch via create_ticket (plain text per the Note Format Standard skill, documentation/note-format-standard) so progress is visible on a board.

## Guardrails

- This skill plans and tracks — it does not perform the migration. The tool surface reads doc platforms; it does not bulk-write them. Content moves via the platforms' import/export tooling, by humans.
- Credentials migrate under credential-storage-audit rules: rotate anything found in plaintext along the way; never reproduce a secret in the plan or tickets.
- Tier 3 requires human sign-off before anything is declared not-migrating — the tier list is a recommendation until the requester approves it.
- No decommission recommendation until Tier 1 verification is at 100% and recorded. "It's probably all there" has ended careers.
- Result-cap honesty: state clearly which inventory counts are capped/sampled — an inventory that silently missed a category is worse than none.
- Degradation: platforms not on the tool surface (many wikis, file shares) are inventoried from the requester's description and flagged as unverifiable by this skill; Notion access requires the member's connection and reflects only that member's visibility.
