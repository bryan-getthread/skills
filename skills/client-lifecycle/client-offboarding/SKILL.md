---
name: Client Offboarding
description: Run a clean client exit — data handover, access revocation on both sides, final billing notes, and the documentation package — as a tracked checklist that protects both parties.
category: Client Lifecycle
tools: [search_clients, search_contacts, search_tickets, create_ticket, update_ticket, list_boards, search_itglue, search_hudu]
---

# Client Offboarding

A departing client deserves a professional exit, and the MSP needs a defensible one. This runbook drives the four workstreams of a clean handover and reports what is done, pending, and at risk — without performing any destructive action itself.

## When to use

- "<client> is leaving — start the offboarding."
- "Run the offboarding checklist for <client>, last day is <date>."
- "What do we still owe <departing client> before cutoff?"

## Steps

1. Confirm the client with search_clients, the contractual end date, and who the receiving party is (new provider or internal IT). If the termination is not yet confirmed by the AM or management, stop — offboarding does not start on rumor.
2. Sweep open work: with search_tickets, list every open ticket for the client. Each needs a disposition — resolve before exit, hand over with context, or close as won't-do with client agreement. No silent closes.
3. Drive the four workstreams, creating one tracking ticket per workstream with create_ticket (confirm the list with the requester first):
   - **Data handover.** Inventory what the client owns and must receive: documentation exports, credential lists (via a secure channel, never in a ticket), backups or backup ownership transfer, license and asset inventories, mailbox/data exports where contracted. Record what was delivered, to whom, and when.
   - **Access revocation — theirs.** Everything of ours the client's people can reach: portal logins, shared channels, support addresses. Schedule removal for the cutoff date, not before.
   - **Access revocation — ours.** Everything of the client's we can reach: admin accounts, RMM agents, remote access, stored credentials, API keys, MFA registrations. Build the removal list from documentation (search_itglue / search_hudu) plus ticket history; execution happens at cutoff by a technician, and each removal is logged.
   - **Final billing notes.** Compile for finance: last billable period, unbilled time from search_tickets, contracted offboarding fees, equipment to recover or transfer title on. Plain-text notes; flag disputes rather than adjudicating them.
4. Assemble the **documentation package** summary: what the receiving party gets, keyed to the data-handover ticket.
5. Report status by workstream — Done / Pending / Blocked — plus a cutoff-day checklist of the actions that must happen on the date itself.

## Guardrails

- Nothing destructive is executed by this skill: no access is revoked, no data deleted, no agents uninstalled. It builds the lists, schedules the tasks, and tracks completion by humans.
- Timing discipline: revoking our own access too early strands the client mid-transition and looks retaliatory; too late is a security exposure. Everything keys off the confirmed cutoff date.
- Credentials never appear in tickets, notes, or this report — reference the secure handover channel only.
- Client data is handed over, never withheld as leverage. Billing disputes are flagged to management, and data handover proceeds unless management explicitly directs otherwise.
- Keep every interaction professional and factual — exit-period notes may be read by lawyers on both sides. No commentary about the departure.
- Confirm before bulk ticket operations; closing the client's open tickets requires explicit sign-off per the disposition list.
- Notes destined for PSA sync are plain text.
