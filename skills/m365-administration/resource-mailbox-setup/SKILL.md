---
name: Resource Mailbox Setup
description: Create and configure room or equipment mailboxes — booking policies, auto-accept vs delegate approval, recurring-meeting and duration limits — so bookings behave the way the client expects. Use when a ticket asks for a bookable meeting room, projector, vehicle, or "why does the room double-book / never respond."
category: M365 Administration
tools: [search_tickets, search_contacts, search_clients, search_knowledge_base, search_itglue, add_ticket_note, send_approval, log_time_entry, web_search]
---

# Resource Mailbox Setup

Ships a resource mailbox whose booking behavior is a deliberate policy, not
the defaults: who can book, whether a human approves, how far out and how long
bookings can run — all confirmed with the client and written down.

## When to use

- "Set up a calendar for Conference Room A / the pool car / the loaner
  laptop."
- "The room accepts everything and double-books" or "booking requests just
  sit there" (existing resource misbehaving).
- Adding delegate approval to an existing room.

## Steps

1. Confirm type and booking policy with the client before creation — these
   are the questions the defaults silently answer wrong:
   - Room or Equipment? (Rooms get locations in Room Finder; equipment
     doesn't.)
   - Auto-accept, or delegate approval? Fully automatic is fine for ordinary
     rooms; anything contended or expensive usually wants a human approver.
   - Who may book: everyone, or a restricted group?
   - Recurring meetings allowed? Standing bookings are how rooms get squatted.
   - Booking window (how far ahead) and maximum duration?
2. Prepare creation for the tech (verify against current module versions):
   - `New-Mailbox -Room -Name "<room>"` or `New-Mailbox -Equipment -Name
     "<item>"` — no license needed under 50 GB; sign-in on the account stays
     blocked, same rule as shared mailboxes.
3. Prepare the booking policy via `Set-CalendarProcessing` per the answers:
   - Auto-accept: `-AutomateProcessing AutoAccept` (this is what prevents
     double-booking — AutoAccept declines conflicts).
   - Delegate approval: `-AllBookInPolicy $false -AllRequestInPolicy $true
     -ResourceDelegates <approvers>` — requests go to the delegates to
     accept/decline. Confirm the delegates actually agreed to the job; an
     approval queue nobody watches is the "requests just sit there" ticket.
   - Restricted booking: `-BookInPolicy <group>` with `-AllBookInPolicy
     $false` (maintain the group, not a person list).
   - Limits: `-AllowRecurringMeetings`, `-BookingWindowInDays`,
     `-MaximumDurationInMinutes` per the client's answers.
   - Consider `-AddOrganizerToSubject`/`-DeleteSubject` defaults: hiding
     subjects protects privacy, showing them helps front-desk staff — ask.
4. For "room misbehaves" tickets, pull the current `Get-CalendarProcessing`
   output first and diff it against intended behavior — double-booking is
   almost always `AutomateProcessing` not set to AutoAccept, and silent
   requests are almost always delegates missing or gone (departed user as
   sole ResourceDelegate is a classic).
5. Get client sign-off (send_approval) on the policy summary — booking rules
   are user-visible behavior for everyone who schedules meetings.
6. Verify via evidence: a test booking auto-accepts (or routes to the
   delegate), a conflicting booking declines, and an over-limit booking
   declines with the policy reason. Post a plain-text note: resource name and
   address, type, full policy (approval mode, who can book, window, duration,
   recurrence), delegates, approver, date, and rollback (prior
   CalendarProcessing values captured in step 4, or defaults for new).
   Log time.

## Guardrails

- Never leave a contended resource on silent defaults — the booking policy is
  confirmed with the client, not assumed.
- Delegate-approval mode requires named, willing delegates; verify they exist
  and are active before enabling it, and flag single-person approval queues
  as fragile.
- Sign-in on resource accounts stays blocked; nobody logs in as the room.
- Capture prior CalendarProcessing settings before changing an existing
  resource — that is the rollback.
- PowerShell labeled: verify against current module versions.
