---
name: Zapier GitHub Escalation
description: For dev-shop clients — escalate reproducible bugs from tickets into the client's GitHub issues with a dedupe-first search and a minimal-repro write-up, then track the issue back to the ticket. Use for "file this bug in their GitHub", "the client's app is broken and it's their code", or dev-shop clients who take bug reports as issues.
category: Connectors
tools: [search_tickets, add_ticket_note, update_ticket, "zapier: GitHub"]
---

# Zapier GitHub Escalation

When a dev-shop client's own software is the problem, the fix lives in their repo — and the desk's job is to hand their engineers a bug report worth reading: deduped against existing issues, reproducible in numbered steps, expected-vs-actual stated, environment pinned. One good issue beats five "app broken again" tickets forwarded raw.

## When to use

- "<client>'s app throws this error for their users — file it with their dev team on GitHub."
- The desk has isolated a reproducible bug in client-owned software.
- "Check if their GitHub already has this bug before we file it."
- Relaying the issue's progress back onto the waiting ticket(s).

## Steps

1. **Verify before promising (GitHub's Zapier inventory is not pre-verified in this library):** run the Zapier Action Discovery pattern first — confirm actions equivalent to "Create Issue", "Create Issue Comment" (or update), and a "Find Issue" / issue-search lookup exist and are enabled for the client's repo before describing the workflow. No search action → dedupe degrades to asking the client's dev lead; say so.
2. Confirm the target repo and the client's issue conventions (labels like "bug"/"from-support", a template, who to mention) — from the escalation agreement or by asking, never by guessing among their repos.
3. **Minimal-repro discipline — earn the escalation:** before filing, the ticket must yield (a) numbered reproduction steps a developer can follow, (b) expected vs actual behavior, (c) environment (app version, OS/browser, account type — sanitized), and (d) frequency/scope (one user or many). Can't reproduce it → it isn't ready to escalate; keep investigating or hand the member the gap ("we need X to make this filable").
4. **Dedupe first, always:** search their issues (open first, then recently closed) for the same error text/symptom. Found open → don't file a twin; add a comment with the new occurrence's evidence and affected-user count, and link the ticket to the existing issue. Found recently-closed → comment asking whether it regressed rather than opening a duplicate.
5. Compose the issue: title in "<symptom> when <action>" form, body with repro steps, expected/actual, environment, first-seen date, and the Thread ticket reference. Client-safe and sanitized: no end-user personal data, no credentials, no internal desk commentary. Show for confirmation, then create with the agreed labels.
6. Capture the issue number/URL and `add_ticket_note` (plain text) on every affected ticket — the escalation handle. Set tickets to the desk's waiting-on-vendor/third-party status via `update_ticket` where one exists.
7. Track back: on request or per pass, read the issue's state and relay material changes (triaged, fix merged, released) onto the waiting ticket(s) with "per GitHub issue #<n>" provenance. Fix released → verify on the desk side before resolving tickets; "merged" is not "fixed for the user".

## Guardrails

- **Member-connected Zapier required** (access to the client's GitHub org/repo, standing and client-granted). Degrade: produce the finished issue body as formatted text for the client's dev lead (or the member) to file, and still record the escalation and its pending status on the ticket.
- Dedupe-first is mandatory: engineering teams triage duplicates as noise, and the desk's escalation credibility is spent one duplicate at a time.
- Their repo: file and comment only. Never close, label-shuffle beyond the agreed labels, assign their engineers, or edit their other issues.
- Sanitization is stricter here than usual — GitHub issues are often visible to the client's whole engineering org (or public): no end-user names/emails (say "an affected user"), no tokens/URLs with secrets, screenshots only if scrubbed.
- Never promise the client's users a fix timeline the issue doesn't state; relay their team's own words with provenance.
- Task cost: each Zapier call = 2 Zapier tasks — one search + one create/comment per escalation, one read per status pass; no polling their repo.
