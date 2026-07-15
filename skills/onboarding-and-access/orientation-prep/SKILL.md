---
name: Orientation Prep
description: Cross-reference a new hire's device-setup and account tickets into one consolidated day-one note — hostname, contact info, and secure credential-handoff status in one place. Use before a hire's start date or orientation session.
category: Onboarding & Access
tools: [search_tickets, search_contacts, search_clients, add_ticket_note, log_time_entry]
---

# Orientation Prep

Saves the day-one scramble: pulls the hire's scattered tickets (account
provisioning, device setup, licenses, shipping) into a single consolidated
readiness note, so whoever runs orientation knows exactly what is ready, what
is not, and how the credentials will be handed over.

## When to use

- "<user> starts tomorrow — are we actually ready?"
- Prepping the orientation session or day-one walkthrough for a new hire.
- A dispatcher's pre-start-date readiness sweep on the onboarding board.
- The onboarding ticket is done but hardware shipped separately and nobody has
  the full picture.

## Steps

1. Anchor the hire: name, client, start date (search_contacts, the onboarding
   ticket). Then find every related ticket with search_tickets — account
   creation, device setup/imaging, license assignment, shipping/logistics,
   phone/telephony — searching per signal (name, onboarding ticket reference,
   client + timeframe), since one query may miss siblings. If searches may
   have hit a result cap, say so in the note rather than implying the sweep
   was exhaustive.
2. From the device ticket(s), extract: hostname/asset tag, device model,
   imaging/enrollment status, shipping status and tracking if remote, and any
   peripherals.
3. From the account ticket(s), extract: account/UPN readiness, mailbox status,
   license assignment, group membership status, and MFA enrollment state
   (enforced at first sign-in?).
4. Credential handoff — status only, never content: HOW the temporary
   credential will reach the hire (secure link sent, manager hands over
   in-person, read over verified call) and whether that handoff has happened.
   If any ticket shows a password in plain text, flag it as a security issue
   to be rotated — do not repeat the value.
5. Cross-check for contradictions: device shipped to the wrong site, account
   ready but license missing, start date moved on one ticket but not the
   others. Each contradiction is a flagged line with the tickets involved.
6. Post one consolidated plain-text note on the primary onboarding ticket:
   hire, start date, device (hostname, status, location), account (status,
   MFA), credential-handoff status, contact info for day-one (manager,
   assigned tech), and a READY / NOT READY verdict with the blocking items and
   owners. Log time (log_time_entry).

## Guardrails

- Never include a password, TAP, or one-time code in the note — handoff
  status only.
- Report only what the tickets actually say; a device is not "ready" because
  its ticket is closed — look for the confirming detail, and mark UNKNOWN
  where it is absent.
- Do not invent hostnames, tracking numbers, or ticket references.
- State result caps honestly when the sibling-ticket sweep may be incomplete.
- Contradictions get flagged, not silently resolved — the tickets' owners
  decide which value is true.
