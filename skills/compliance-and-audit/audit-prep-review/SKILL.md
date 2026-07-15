---
name: Audit Prep Review
description: An audit is coming — run the pre-audit sweep for unresolved prior findings, documentation gaps, and stale evidence, and return a ranked readiness report before the auditor finds it first.
category: Compliance & Audit
tools: [search_tickets, search_itglue, search_knowledge_base, add_ticket_note]
---

# Audit Prep Review

The point of audit prep is to find your own gaps before the auditor does. This sweep checks the three places audits go wrong — unresolved prior findings, undocumented or stale documentation, and evidence past its freshness date — and ranks what to fix in the time remaining.

## When to use

- "The audit is in <N> weeks — what's our exposure?"
- A compliance owner wants a readiness check against a framework's expectations
- Post-remediation: verifying prior findings were actually closed

## Steps

1. Fix the scope: which client/environment, which framework or audit type, and the audit date — the date sets how the findings get ranked (fixable-in-time first).
2. Prior findings sweep: search_tickets for the previous audit's findings and their remediation tickets. Classify each as closed-with-evidence, closed-without-evidence (a finding in itself — "done" with nothing to show reopens badly), in progress, or untouched. Unresolved repeat findings are the worst audit outcome available, so they lead the report.
3. Documentation freshness sweep: check the in-scope policies and procedures (search_itglue, search_knowledge_base) for last-reviewed dates outside the review cycle, owners who have left, and procedures that no longer match practice (a documented process the tickets show nobody follows is a gap wearing a costume).
4. Evidence freshness sweep: from the ticket record, the last completed access review, the last backup-restore test, the last DR exercise, the last incident-response test — each with its date and whether it falls inside the framework's expected cadence. Date-stamp every check; "recent" is not a date.
5. Open-exceptions sweep: security exceptions, risk acceptances, and change-gate bypasses (emergency changes without retroactive documentation, via the change-request-prerequisites lens) that an auditor will sample.
6. Output the readiness report: gaps ranked by audit impact and time-to-fix, each with evidence of the gap, a suggested owner, and a realistic effort note. Post as a plain-text note for the compliance owner. Remediation itself is theirs to assign — the report recommends, it does not act.

## Guardrails

- Report gaps honestly and completely — softening the internal readiness report defeats its entire purpose; the auditor will not soften theirs.
- No silent remediation: this skill changes nothing, closes nothing, and backfills nothing — it reports. Fabricating or backdating evidence to close a gap is never on the table.
- Absence claims carry search scope and result-cap honesty: "no restore-test ticket found in <period> across <boards>."
- Date-stamp every freshness check; the report itself carries its as-of date because readiness decays.
- Findings about people's unfinished work go to the compliance owner, not into blame narratives — process focus, per the postmortem discipline.
