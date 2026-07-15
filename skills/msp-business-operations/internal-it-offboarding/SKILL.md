---
name: Internal IT Offboarding
description: Offboard departing MSP staff — client-credential rotation first (they had keys to every environment), then tool deprovisioning, ticket reassignment, and client-facing transition notes. Use when the leaver works FOR the MSP, not for a client.
category: MSP Business Operations
tools: [search_tickets, search_members, search_clients, search_knowledge_base, search_itglue, search_hudu, create_ticket, update_ticket, add_ticket_note, send_approval, log_time_entry]
---

# Internal IT Offboarding

Runs a staff departure as what it actually is at an MSP: a multi-tenant security event. A departing technician held credentials into client environments — shared admin accounts, RMM consoles, VPNs, the password vault — so rotation of client-facing access comes before anything else, then internal deprovisioning, then a clean handover of their tickets and client relationships. This is the internal twin of the client-facing employee-offboarding skill, with a wider blast radius.

## When to use

- "Offboard <user> — last day is Friday." / "Terminate <user>'s access effective now."
- An immediate-exit situation where access must be cut same-hour.
- Auditing a past departure: "did we actually rotate everything when <user> left?"
- Building the internal offboarding checklist itself.

## Steps

1. Confirm the essentials: the leaver, last working day, and whether this is a standard exit (planned, cooperative) or an immediate one (access cut now, questions later). The mode changes the ordering urgency, not the checklist.
2. **Client-credential rotation FIRST.** Build the exposure inventory before touching internal accounts:
   - Their vault/folder access in the password manager — every shared credential in scope is now rotation-eligible.
   - Shared and named admin accounts in client environments (domain admin, M365 global admin, firewall, hypervisor) they could use or knew.
   - RMM/remote-access reach: which client sites their console account or agent access covered.
   - VPN profiles, client-issued accounts, and any client that gave them a personal login.
   Pull the map from the documentation platform (search_itglue / search_hudu / search_knowledge_base) and their ticket history (search_tickets) for clients they worked heavily. Post the inventory as a rotation checklist on the offboarding ticket and track each item to done. Shared credentials are rotated, not just "removed" — removal doesn't help if they memorized the password.
3. Internal account cutover, in the same order discipline as a client offboarding: identity sign-in blocked and sessions revoked first, then MFA methods cleared, then mailbox handled (delegate to manager before any license removal), then chat/collaboration.
4. Tool deprovisioning and seat reclaim: PSA/Thread member account deactivated (after ticket reassignment — see step 5), RMM console user removed, documentation platform user removed, remote-access licenses reclaimed. Note each reclaimed seat — this is real money at renewal.
5. Work handover:
   - Reassign their open tickets (search_tickets for open work owned by the leaver; update_ticket to the covering tech or the dispatch queue) with a one-line context note per ticket where the thread doesn't speak for itself.
   - Identify clients where the leaver was the named or de-facto primary contact, and draft the client-facing transition note: warm, brief, forward-looking — "your primary engineer is now <user>; nothing else about your service changes." Never state the reason for departure; never editorialize.
6. Close with an audit note on the ticket: rotation checklist complete, accounts disabled with timestamps, seats reclaimed, tickets reassigned, clients notified. This note is what a future "did we rotate everything?" audit reads.

## Guardrails

- **Rotation before deprovisioning, always.** Disabling their PSA login while client domain-admin passwords they knew stay live is the failure mode this skill exists to prevent.
- You inventory, sequence, and track; authorized admins execute the rotations and disables. Do not present the checklist as done — the zero-assumption rule applies to every line of it.
- Immediate exits: recommend identity block + session revoke + vault removal within the hour, and say explicitly which items remain exposed until rotated.
- No reasons for departure in any note — internal notes stay factual ("offboarded effective <date>"), client notes stay warm and reason-free. Termination circumstances are HR's information, not the desk's.
- Client transition notes are drafts for a human (manager or account manager) to approve and send — do not send them yourself.
- If the vault or docs platform can't produce a reliable access map, say so on the ticket and recommend rotating the standard shared-credential set for every client the leaver's ticket history touched — over-rotation is the safe direction.
- Never post the exposure inventory (which credentials exist where) beyond the offboarding ticket; it is itself sensitive.
