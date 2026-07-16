---
name: Vade Email Security
description: A Vade email-security verdict landed — read the threat classification (phishing/malware/spam/scam), separate what Vade already blocked or quarantined from what's still live, and hold the line on allow-list requests.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# Vade Email Security

Vendor specialization of security-alert-response and phishing-triage for Vade (Vade Secure), the predictive email-security engine often deployed for Microsoft 365. The generic email runbooks own the investigation canon; this skill adds Vade's verdict vocabulary, its remediation model, and the allow-discipline that keeps a convenience exception from reopening the channel. Verify classification names and console layout against Vade's current documentation.

## When to use

- A Vade alert or user report arrives: a message classified phishing, malware, scam, or spam; a quarantined item; or a "why was this blocked / please release it" request
- A tech asks how to read a Vade verdict or whether an item is safe to release
- A recurring false-positive prompts an allow-list request

## Steps

1. Parse the Vade verdict anatomy: classification (phishing / malware / scam/spear-phishing / spam / commercial), the confidence signal, sender and authentication results (SPF/DKIM/DMARC), URL/attachment findings, recipients, and the remediation state — quarantined, banner-warned, or delivered. Copy Vade's exact classification wording into the note.
2. Route to the client per security-alert-response using tenant/domain fields; low routing confidence → flag for a human.
3. Separate done-from-live: a quarantined message is contained for that recipient — confirm by effect, then scope. A banner-warned or delivered message is live: check whether recipients interacted (clicked, replied, entered credentials) per phishing-triage before anything else. Delivered-and-clicked on a credential-harvesting URL → branch to compromised-account-containment for that user.
4. Scope the campaign: search the tenant for the same sender/subject/URL across other mailboxes (search_tickets for prior context, same client + indicator, 90 days) — Vade quarantines per-message, so siblings may still sit in other inboxes; a tenant-wide search/purge is a technician action.
5. Release discipline: a release is a security decision. Release only on confirmed-benign evidence (verified legitimate sender, business-necessary mail wrongly classified) — never on user impatience. Verify the sender through an independent channel, not the reply-to in the suspect mail. Document the evidence and who approved.
6. Allow-list discipline: recurring false positives go through security-noise-tuning — allow the narrowest scope (specific sender address over domain; domain over broad rule), with a named approver and a review date. An allowed domain is a permanent bypass; treat it that way.
7. Document classification, interaction check, scope result, release/allow rationale, and classify per soc-classification-tree. Client-facing wording per defensive-writing-standard.

## Guardrails

- A quarantine covers one recipient; never close a phishing verdict without scoping the whole tenant for siblings.
- Never release or allow on user say-so alone — verify the sender independently; the reply-to address is not verification.
- Allow-listing is a permanent channel bypass: narrowest scope, named approver, review date — "it's a vendor we trust" still gets the narrow scope.
- Never downgrade a phishing/malware verdict to clear a queue; if interaction occurred, the identity path opens regardless of the quarantine.
- The agent has no Vade console or tenant access — quarantine release, tenant-wide purge, and allow-list changes are technician steps the agent directs and records.
- Degradation: without documentation access, the client's legitimate-sender inventory is unknown — lean on independent sender verification and say so.
