---
name: Zapier GitLab Escalation
description: For dev-shop clients on GitLab — escalate reproducible bugs from tickets into the client's GitLab issues with dedupe-first search and minimal-repro write-ups, then track the issue back to the ticket. Use for "file this in their GitLab", client-owned-software bugs at GitLab shops, or relaying GitLab issue progress to waiting tickets.
category: Connectors
tools: [search_tickets, add_ticket_note, update_ticket, "zapier: GitLab"]
---

# Zapier GitLab Escalation

The GitLab twin of the GitHub escalation: when the client's own software is broken and their engineering lives in GitLab, the desk files one deduped, reproducible, well-formed issue in the agreed project — minding GitLab's vocabulary (projects/groups, issues, confidential flag, labels) — and relays progress back to the waiting tickets.

## When to use

- "<client>'s app is failing for their users — raise it with their devs on GitLab."
- A reproducible bug in client-owned software at a GitLab shop.
- "Has their GitLab already got this bug?"
- Bringing waiting tickets up to date from the GitLab issue's state.

## Steps

1. **Verify before promising (GitLab's Zapier inventory is not pre-verified in this library):** run the Zapier Action Discovery pattern first — confirm actions equivalent to "Create Issue", comment/update actions, and an issue search/find lookup exist and are enabled for the client's project. Also check whether the confidential flag is a settable field — it decides whether sensitive repros can be filed at all. Missing search → dedupe degrades to asking their dev lead; say so.
2. Confirm the target project (group/project path) and the client's conventions: labels, issue template, whether support-escalated issues should be marked confidential. From the escalation agreement or by asking — never guessed from their group tree.
3. **Minimal-repro discipline:** the ticket must yield numbered repro steps, expected vs actual, environment (version/OS/browser, sanitized), and scope (one user or many) before anything is filed. Not reproducible → not filable yet; report the gap instead of forwarding noise.
4. **Dedupe first:** search their issues (open, then recently closed) for the symptom/error text. Open match → comment the new evidence and affected count on it, link the ticket, done. Recently-closed match → comment asking about regression rather than opening a duplicate.
5. Compose the issue: "<symptom> when <action>" title, body with repro steps, expected/actual, environment, first-seen date, Thread ticket reference; agreed labels; confidential flag when the repro touches anything sensitive and the flag is verifiably settable. Show for confirmation, then create.
6. Capture the issue IID/URL and `add_ticket_note` (plain text) on every affected ticket; set waiting-on-third-party status via `update_ticket` where the desk has one.
7. Track back: on request or per pass, read the issue and relay material changes (labeled/triaged, MR merged, deployed) onto waiting tickets with "per GitLab issue #<n>" provenance. Merged ≠ fixed-for-the-user: verify on the desk side before resolving.

## Guardrails

- **Member-connected Zapier required** (the client's GitLab group/project, standing client-granted access — self-managed GitLab instances may not be reachable via the Zapier app at all; that's a step-1 finding, not a surprise). Degrade: produce the finished issue text for the client's dev lead or the member to file, and record the pending escalation on the ticket.
- Dedupe-first is mandatory — duplicates burn the desk's credibility with their engineers.
- Their project: file and comment only; never close, reassign, restructure labels beyond the agreed set, or touch their other issues and merge requests.
- Sanitization at engineering-org visibility: no end-user names/emails, no secrets in logs or URLs, scrubbed screenshots only. Prefer the confidential flag when in doubt and available.
- Relay their team's stated progress with provenance; never invent a fix ETA for the client's users.
- Task cost: each Zapier call = 2 Zapier tasks — search + create/comment per escalation, one read per status pass; no polling.
