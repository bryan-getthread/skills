---
name: PSA Taxonomy Cleanup
description: Rationalizing a PSA desk's ticket type / subtype / category sprawl — a manual sweep that censuses real usage from tickets, proposes merges and retirements, and enforces migration discipline before anything is changed.
category: PSA-Specific
tools: [search_tickets, list_boards, list_ticket_statuses]
---

# PSA Taxonomy Cleanup

Applies to: **any PSA** (ConnectWise Manage, Autotask, HaloPSA). Over time a desk's type / subtype / category (and item) lists sprawl — near-duplicates ("Email" vs "E-mail" vs "O365 Email"), one-off values used twice ever, and overlapping buckets that make reporting meaningless. This is a **manual, on-demand** rationalization sweep: census how the taxonomy is *actually used* from real ticket data, propose merges and retirements grounded in that evidence, and — because the PSA is master — enforce migration discipline so no historical ticket is orphaned.

## When to use

- "Our ticket types are a mess — help me clean them up."
- Reporting by type/category is unreliable because values overlap or duplicate.
- Before a reporting project or a new board rollout (`skills/psa-specific/psa-new-board-setup`) that depends on a coherent taxonomy.
- Periodic hygiene on a desk that's accumulated years of ad-hoc values.

## Steps

1. Confirm scope: which board(s) and which dimension(s) — type, subtype, category, item — and the lookback window for the usage census. Taxonomies are often **per-board** (especially CW); do not merge across boards without confirming the values mean the same thing.
2. **Usage census.** Pull tickets over the window with `search_tickets` (one search per value or per board so result caps land per-slice, not globally) and tally how many tickets carry each type/subtype/category value. The census — not intuition — is the evidence base. Flag every count that may have hit a search cap as "at least N".
3. Classify each value from the census: **high-use** (keep), **duplicate/near-duplicate** (merge candidate → name the survivor), **low/zero-use** (retire candidate), **overlapping** (ambiguous bucket needing a definition, not just a merge). Group merge candidates by the surviving value.
4. **Propose, don't execute.** Produce a proposal: the target taxonomy, each merge (from → into) with its ticket count, each retirement with its count, and a one-line rationale per change. Note where a "small" count is actually capped and could be larger.
5. **Migration discipline.** For every merge/retire, spell out what happens to the historical tickets carrying the old value: they must be remapped to the survivor (or explicitly retained read-only), never left pointing at a deleted value. State the order — remap existing tickets first, retire the value second — so no ticket is orphaned. This remap is **PSA-side work** (the PSA is master of the taxonomy); Thread mirrors the result. Flag any value tied to Flow conditions, Views, agreements, or reports that would break if it changes.
6. Output a plain-text cleanup plan: current-state census table (value | count | classification), proposed target taxonomy, the merge/retire list with counts and rationale, the migration order, and a "review before enacting" list of dependencies (Flows/Views/agreements/reports) touched. End with what a human must do in the PSA and in what sequence.

## Guardrails

- **Advisory, read-only.** This skill censuses and proposes; it changes nothing. Recommendations are never converted into completed actions (zero-assumption rule).
- **The PSA is master of the taxonomy.** Merges, retirements, and historical remaps are enacted in the PSA by a human; Thread reflects them afterward. Do not "clean up" Thread-side values independently.
- **No orphaned history.** Never propose retiring a value without a remap (or explicit read-only retention) plan for the tickets that use it. Retiring before remapping breaks reporting on closed work.
- **Result-cap honesty.** A low census count may be a capped search, not a rare value — say "at least N" and split searches per value/board before recommending a retirement.
- **Check dependencies first.** A type/category referenced by a Flow condition, a saved View, an agreement mapping, or a standing report cannot be changed silently — surface every such dependency in the plan.
- Keep the target taxonomy minimal and non-overlapping; each surviving value needs a one-line definition so the sprawl doesn't regrow.
- Plain-text output; no markdown/emojis in anything destined for a PSA note.

## Cross-references

- CW type/subtype/item structure: `skills/psa-specific/cw-type-subtype-item`
- Autotask categories: `skills/psa-specific/autotask-ticket-categories`
- Designing taxonomy for a new board: `skills/psa-specific/psa-new-board-setup`
- Field mapping documentation: `skills/psa-specific/psa-field-mapping-doc`
- General migration hygiene: `skills/psa-specific/psa-migration-hygiene`
