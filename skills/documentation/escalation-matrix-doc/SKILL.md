---
name: Escalation Matrix Doc
description: Build the per-client who-to-call escalation ladder — internal desk tiers plus vendor and client escalation contacts, with the trigger for each rung and a review cadence — so escalation follows a documented path instead of guesswork under pressure.
category: Documentation
tools: [search_clients, search_contacts, search_tickets, search_members, search_itglue, search_hudu, notion-search, notion-create-pages, notion-update-page, add_ticket_note]
---

# Escalation Matrix Doc

Produces the client's escalation matrix: the ordered ladder of who gets pulled in as an issue climbs — internal desk tiers, the client's own approvers and decision-makers, and vendor escalation paths — each with the condition that triggers moving to the next rung. It is the document a tech reads at 2am so escalation is a lookup, not improvisation.

## When to use

- "Who do we escalate to for <client>, and when?"
- Onboarding a client and building the escalation section of the doc pack (see documentation/client-onboarding-documentation-pack) or the DR plan (documentation/disaster-recovery-plan-doc).
- After an incident where escalation stalled because nobody knew who to call next.
- Periodic review — confirming the escalation contacts and triggers are still current.

## Steps

1. Establish the client with `search_clients` and gather the players: `search_contacts` and `search_tickets` for the client-side people who actually approve, decide, and get escalated to (and after-hours contacts); `search_members` for the internal desk tiers/owners; `search_itglue` / `search_hudu` (where enabled) and `notion-search` (if connected) for any existing escalation documentation and vendor escalation paths.
2. Build the **escalation matrix** as an ordered ladder, each rung with its trigger:
   - **Internal tiers** — first-line → senior/specialist → team lead / on-call → management, with the condition that moves an issue up (severity, time without progress, scope, or explicit request). Keep triggers descriptive; note that Thread Flows cannot fire on elapsed time, so any "after N hours" rung is a human judgment call, not an automated escalation.
   - **Client-side** — primary contact → change/spend approver → IT decision-maker → executive sponsor, with when each is engaged (approvals, business-impact decisions, outage notification).
   - **Vendor** — per critical vendor, first-line support → named escalation contact / TAM, with the contract tier and how to invoke priority support (pull details from documentation/vendor-contact-directory).
   - **Severity definitions** — a short shared scale (what counts as critical vs. high vs. normal for this client) so triggers mean the same thing to everyone.
   - For each contact: name, role, contact method, and hours of availability — sourced, not assumed.
3. Flag gaps: rungs with no named contact, vendors with no escalation path on file, and any after-hours coverage hole. These are capture follow-ups.
4. Set the **storage and cadence discipline**: the matrix lives under the client's section in the desk's documentation platform, dated, reviewed on cadence and when contacts change. Confirm the location convention rather than inventing one.
5. Output the matrix as a paste-ready table/ladder plus the gap list. If the desk keeps escalation docs in Notion and the destination is confirmed, create with `notion-create-pages` or refresh with `notion-update-page`; otherwise paste-ready blocks. Attach to the onboarding/review ticket via `add_ticket_note` if requested.

## Guardrails

- **Sourced contacts only** — names, roles, and numbers trace to contacts, tickets, or docs. Never invent an escalation contact or phone line; unknowns are flagged for capture.
- Triggers are descriptive judgment aids, not automation — Thread Flows have no elapsed-time trigger, so do not describe time-based rungs as auto-firing.
- Neutral, professional wording for any handling notes about people — no insults or characterizations (see documentation/client-wiki-page conventions).
- **No credentials** in the matrix; vendor PINs/portal logins are referenced by vault entry title only (see documentation/credential-storage-audit).
- Notes and tables are plain text per the Note Format Standard (documentation/note-format-standard) — they may sync to a PSA.
- Docs tools are search-only; writes go to Notion only when the member has connected it and confirmed the destination, otherwise paste-ready output. Degradation: without IT Glue/Hudu, build from contacts, members, and ticket history and mark the vendor/after-hours rungs partial.
