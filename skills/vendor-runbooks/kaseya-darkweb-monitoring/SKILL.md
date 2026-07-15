---
name: Kaseya Dark Web Monitoring
description: A Dark Web ID (Kaseya) compromise alert arrived — parse the vendor's alert anatomy (source, date, data classes), then run the dark-web-alert-lifecycle age/notify logic. Same hard no-cracking policy.
category: Vendor Runbooks
tools: [search_tickets, search_contacts, add_ticket_note, update_ticket, view_openDraft]
---

# Kaseya Dark Web Monitoring

Vendor specialization of dark-web-alert-lifecycle for Kaseya's Dark Web ID (and its BullPhish-adjacent reporting). The lifecycle logic — age it, close stale with a note, notify fresh with rotation guidance — lives in the generic skill; this adds how Dark Web ID structures its compromise records and the fields that decide the stale-vs-fresh branch. The no-cracking policy applies identically and without exception. Verify field names against the vendor's current report format.

## When to use

- A Dark Web ID compromise alert or scheduled compromise report lands as a ticket
- A batch of Dark Web ID findings for a monitored domain needs working
- Embedded in a Flow that processes the dark-web alert board

## Steps

1. Parse the Dark Web ID record anatomy: the monitored identity (email address under the client's watched domain), the compromise source (named breach, or generic labels like "ID theft forum" / "botnet" — botnet/infostealer sources are the hot ones), the date found vs the breach date (use the older, breach-origin date for aging; "date found" is when the feed saw it, not when it leaked), password visibility (visible plaintext, partially masked, hash, or blank), and any PII classes attached.
2. Run dark-web-alert-lifecycle on those fields: parseable origin date older than 90 days → stale-path closure with the documented note (source, date, data classes, rationale, prior-ticket reference via search_tickets). No parseable date → no auto-close; treat as fresh or leave for a human.
3. Vendor-specific escalators on the fresh path:
   - Source labeled botnet/infostealer → the credential likely came off a currently- or recently-infected device, not an old third-party breach: in addition to rotation, the user's devices need an EDR/AV review (edr-detection-runbook on their device) before or alongside the reset — otherwise the stealer re-harvests the new password.
   - Visible plaintext password that matches the client's current password pattern or the user confirms is current → breached-credential-response immediately; any sign of use → compromised-account-containment.
   - Findings for departed employees → route to the client contact per the generic skill; flag still-active accounts.
4. Batch reports: work per-identity, not per-report — one report row per exposed identity gets its own verdict; do not close a whole report because most rows are stale. State result caps honestly if the report was truncated.
5. Document per the generic skill: verdict, age math, source class, data classes, what was sent to whom. Classify per soc-classification-tree.

## Guardrails

- POLICY (identical to dark-web-alert-lifecycle, no vendor exception): never attempt to crack, decode, or look up leaked passwords or hashes on external sites or tools; never test a leaked credential by signing in; treat any exposed hash as a compromised password and rotate.
- Never include the exposed password/hash value in client-facing messages — identify by service and date.
- "Date found" is not the breach date — aging on the wrong field auto-closes fresh botnet finds; when only "date found" exists and the source is botnet/infostealer, treat as fresh.
- Stale-path closure requires a parseable origin date — no date, no auto-close.
- Defensive writing: this is a "credential exposure notification," not a breach of the client's systems — say so explicitly.

## Unattended (Flows) variant

- Inherits the dark-web-alert-lifecycle variant verbatim: entire reply is the plain-text internal note; only stale-close and fresh-triage-note outcomes are autonomous; never send client email autonomously; botnet/infostealer source, missing date, or any parsing doubt → do nothing and leave for a human.
