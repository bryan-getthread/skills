---
name: IRONSCALES Phishing
description: An IRONSCALES incident or user banner-report landed — read the mailbox-level detection and classification, act on the automated remediation across affected mailboxes, and triage employee reports without training the model wrong.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# IRONSCALES Phishing

Vendor specialization of security-alert-response and phishing-triage for IRONSCALES, the mailbox-level anti-phishing platform. Two things define it: detection sits *inside* the mailbox (so it sees post-delivery threats and can remediate across every affected inbox at once), and its in-mailbox banner lets users report suspected phishing with one click — which means a stream of employee reports of varying quality. The generic email runbooks own the canon; this skill adds IRONSCALES' incident model and report-triage discipline. Verify feature and classification names against IRONSCALES' current documentation.

## When to use

- An IRONSCALES incident fires: a classified phishing/malicious/suspicious campaign, or an auto-remediation across mailboxes
- A user banner-report ("Report Phishing") comes through for analyst decision
- A tech asks how to read an IRONSCALES verdict or how far a remediation reached

## Steps

1. Parse the incident anatomy: classification (phishing / malicious / suspicious / spam / safe), the themis/AI confidence signal, sender and authentication results, URLs/attachments, the number of affected mailboxes in the campaign cluster, and the remediation state — auto-remediated (pulled from inboxes), pending analyst decision, or report-only. Copy IRONSCALES' exact wording.
2. Route to the client per security-alert-response using tenant/domain fields; low confidence → flag for a human.
3. Use the mailbox-level advantage: IRONSCALES clusters the same campaign across every recipient. Confirm the remediation reach — did the pull cover all clustered mailboxes, or only some? Any un-remediated recipient is live; check interaction (click/reply/credential entry) per phishing-triage. Delivered-and-clicked on credential harvesting → compromised-account-containment for that user.
4. Triage banner-reports proportionately: a user report is a signal, not a verdict. Classify on evidence, not on the reporter's alarm — but never dismiss silently, because the classification trains the model and shapes the user's future reporting. A true positive gets remediated tenant-wide and the reporter acknowledged; a false alarm gets a clear, kind explanation.
5. Scope beyond the cluster: search prior context (search_tickets, same client + sender/URL, 90 days) for the same actor across earlier campaigns.
6. Classification discipline: marking a message safe/malicious in IRONSCALES feeds the AI — treat a "safe" verdict with the same evidence bar as closing the ticket, and never mark safe to silence a noisy reporter.
7. Document classification, remediation reach, interaction check, report-triage outcome, and classify per soc-classification-tree. Client-facing wording per defensive-writing-standard.

## Guardrails

- Auto-remediation reach is a claim until confirmed — verify every clustered mailbox was covered before closing; a partial pull leaves live copies.
- Never mark a message safe to quiet a frequent reporter — it mistrains the model and blinds the desk to the next real one.
- User banner-reports get a genuine decision and a response, never silent dismissal — reporting behavior is a security asset worth protecting.
- Interaction (click/reply/credential) opens the identity path regardless of whether the message was later remediated.
- The agent has no IRONSCALES console access — remediation, classification changes, and tenant purges are technician steps the agent directs and records.
- Degradation: without documentation access (search_itglue), the client's mailbox scope and licensed features may be unknown — say so.
