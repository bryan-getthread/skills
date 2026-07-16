---
name: Docs Owner Monthly Ritual
description: A documentation owner's monthly runbook — stale-doc hygiene, KB coverage report, doc-gap sweep, and runbook-testing schedule check — run when the docs owner says "monthly docs pass", "documentation hygiene", or "how healthy is the KB".
category: Role Rituals
tools: [search_knowledge_base, search_tickets, search_itglue, search_hudu]
---

# Docs Owner Monthly Ritual

The documentation owner's monthly maintenance cycle: age out what's stale, measure what's covered, find what's missing, and make sure the runbooks people trust have actually been tested. ~2 hours, monthly.

## When to use

- "Run the monthly docs pass" / "KB health check" / "documentation hygiene time."
- Start of month, on the docs owner's maintenance slot.
- Before an audit or onboarding wave where doc quality gets exposed.

## Steps

1. **Stale-doc hygiene (30 min)** — run `skills/documentation/stale-doc-hygiene`: docs past their review age or contradicted by recent tickets. Disposition each: refresh, archive, or reassign to the tech who touched the subject last. Archive is a valid outcome — a wrong doc is worse than no doc.
2. **KB coverage report (30 min)** — run `skills/documentation/kb-coverage-report`: coverage by client and by issue category, movement since last month, and the thin spots.
3. **Doc-gap sweep (30 min)** — run `skills/documentation/doc-gap-detector` over the month's tickets: issues techs solved from memory or vendor calls with no doc to lean on. Cross-rank with step 2's thin spots; the top gaps become assigned stubs (owner + due date), drawing candidates from `skills/documentation/sop-candidate-finder`.
4. **Runbook-testing check (20 min)** — run `skills/documentation/runbook-testing-schedule`: which runbooks are overdue a test-run, especially anything incident-critical (restore procedures, failover, security response). Overdue critical runbooks get a scheduled test with a named tester.
5. Output the monthly docs card: hygiene dispositions (refreshed/archived/reassigned counts + notable items), coverage movement, top gaps with owners and dates, and the runbook test schedule for the coming month.

## Time-short version

Steps 1 and 4 — actively wrong docs and untested critical runbooks are the two liabilities. Coverage and gap-mining compress into "top three gaps by ticket volume" with a note.

## Guardrails

- Never delete — archive with a reason, so a mistaken staleness call is reversible.
- Gap assignments go to techs who actually worked the tickets, with due dates the docs owner tracks; unowned gaps are re-listed next month, not forgotten.
- Doc changes touching credentials or security procedures route through `skills/documentation/credential-storage-audit` conventions rather than casual edits.
- Degrade gracefully per platform: if IT Glue/Hudu isn't connected for this tenant, run the ritual over the native KB and say so.
- Skip-with-note beats fake completion; disclose result caps on month-scale ticket mining.
