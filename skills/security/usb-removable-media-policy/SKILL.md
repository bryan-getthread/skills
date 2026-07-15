---
name: USB Removable Media Policy
description: A user is asking for an exception to the USB/removable-media block — evaluate the business justification, grant it scoped and time-boxed with approval, enforce scan-before-use, and never quietly widen the policy.
category: Security
tools: [search_tickets, search_contacts, add_ticket_note, update_ticket, send_approval, schedule_ticket]
---

# USB Removable Media Policy

USB blocks exist because removable media is both an infection vector and an exfiltration channel — and exceptions are where blocks go to die. This skill handles exception requests the durable way: real justification, narrowest workable scope, an expiry date that actually fires, and a scan discipline for the media itself.

## When to use

- "I need to use a USB drive and it's blocked" lands as a ticket
- A recurring business process needs removable media (field data collection, machine-tool transfer, AV equipment)
- An expiring exception comes up for renewal, or a lead asks to review the exception list

## Steps

1. Understand the need, not the request: what data or task, with whom, how often, and — the question that resolves most tickets — is there an approved alternative that works? A one-time file exchange is usually better served by the client's sanctioned sharing method (managed cloud share, secure transfer) than by an exception. Offer the alternative first; a solved need with zero exceptions is the best outcome. Genuine cases exist — air-gapped equipment, vendor-supplied media, field devices — and proceed.
2. Verify the requester and check history: search_contacts for role (does the need match the job?), search_tickets for prior exceptions for this user or process — a pattern of repeating "one-time" exceptions means the process needs a permanent sanctioned path decided by the client, not a fourth temporary grant. If the request context carries exfiltration-shaped signals (departing employee, bulk data, DLP history), route through dlp-alert-triage / insider-risk-basics before ANY grant — an exception request can be an exfiltration plan with a ticket number.
3. Scope the exception as narrowly as the need supports, in order of preference: specific registered device (by serial/hardware ID) for a specific user on a specific machine → read-only access where the need is import-only (kills the exfil half entirely) → user-level allowance only when device-specific is impractical. Never machine-wide "USB unblocked," never group-wide grants for one person's need.
4. Time-box everything: every exception carries an expiry — days for one-offs, a defined review date (e.g. quarterly) for standing business processes. No "permanent" exceptions: standing needs get standing *reviewed* exceptions. Schedule the expiry/review (schedule_ticket) so it fires — an exception without a firing expiry is a policy change wearing a ticket costume.
5. Approval gate before implementation: the exception, its scope, duration, and justification go for approval per the client's policy — their designated authority for security exceptions (send_approval where available). No approval, no exception; verbal doesn't count. The technician implements in the endpoint-management/DLP console; the agent records scope, approver, implementation timestamp, and expiry in a plain-text note.
6. Scan-before-use discipline, stated in the grant: media is scanned by the endpoint's protection before files are opened (and on-insert scanning verified as enabled on the excepted machine); vendor-supplied or unknown-origin media gets scanned on a designated station, not on production machines of opportunity; found-in-the-parking-lot media is never inserted anywhere — it goes to the security desk. Include the one-line user version in the ticket reply.
7. Lifecycle: at expiry the exception is REMOVED and its removal verified (not assumed — check the console state), with a renewal path if the need persists. Periodic review sweeps the full exception list against these rules; anything unscoped, unexpiring, or unowned gets remediated. Repeat-need patterns go to the client as a process recommendation.

## Guardrails

- Every exception: scoped, justified, approved, time-boxed, and recorded — a grant missing any of the five is not granted.
- Expiry means removed-and-verified; unexpired-forever exceptions are audited as policy violations, not grandfathered.
- Never widen silently: one user's exception never becomes a machine, group, or client-wide unblock as a convenience; policy-level changes are the client's documented decision.
- Exfiltration-shaped requests get the insider-risk lens BEFORE the grant, handled per that skill's no-tip-off discipline.
- Alternatives first: recommending the sanctioned path is success, not obstruction — but say it helpfully, the requester has a job to do.
- The agent evaluates, routes approval, and records; the technician executes console changes.
- Degradation: without send_approval, route approval through the client's documented channel and record the written approval in the ticket before implementation.
