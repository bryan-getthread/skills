---
name: Journaling & Compliance Mail
description: Handle journaling and compliance-copy requests — legal/regulatory justification required, journal target must be external to the tenant, storage and third-party-archive implications costed, and retention/hold offered first where it fits. Use when a ticket asks to journal mail, BCC-copy all messages somewhere, or feed an external compliance archive.
category: M365 Administration
tools: [search_tickets, search_clients, search_knowledge_base, search_itglue, add_ticket_note, send_approval, log_time_entry, web_search]
---

# Journaling & Compliance Mail

Implements message journaling only when it is actually the right tool — with
the legal justification on record, the target architecture valid (Exchange
Online cannot journal to itself), and the client aware of what the copy
stream costs and exposes.

## When to use

- "We need to journal all mail for FINRA/SEC/insurance compliance."
- "BCC every message to/from <department> to an archive."
- "Set up the feed to our compliance archive vendor."
- "Do we still need this old journal rule?" (review/retire path).

## Steps

1. Justification gate first: journaling copies every in-scope message —
   including personal and privileged content — to another system. Require a
   documented legal/regulatory driver from the client's compliance or legal
   authority (send_approval), naming the regulation or matter and the
   required scope and duration. "It'd be nice to have copies" does not
   clear this bar; a manager wanting to read a team's mail clears no bar
   at all — decline and point to proper channels.
2. Check whether retention is the better tool before building journaling:
   Microsoft's direction is retention policies + litigation hold + eDiscovery
   for most preservation needs (retention-policy-requests, litigation-hold).
   Journaling wins only when a REGULATOR requires an immutable copy stream
   to an independent archive, or a third-party archive vendor is mandated.
   Present the comparison to the client; retention avoids the external
   dependency entirely.
3. Architecture constraints — state these before design:
   - Exchange Online cannot journal to a mailbox in the same Exchange Online
     organization. The journal target must be external: a third-party
     archive service address or an on-prem/other-system mailbox.
   - Configure the undeliverable-journal-report mailbox (required by the
     journaling setup): if the journal target rejects mail, reports pile up
     there — it must be monitored, or journaling fails silently, which for
     a regulated client is the worst outcome.
   - An archive mailbox is not a journaling target and auto-expanding
     archives explicitly don't support journaling use
     (archive-mailbox-enablement).
4. Scope least-broadly: journal rule scope is per-recipient/sender group or
   global, and direction (internal/external/all). Journal only the population
   the regulation covers (the licensed reps, the named department), not the
   whole tenant "to be safe" — every extra mailbox journaled is extra
   storage cost at the archive vendor and extra privacy exposure.
5. Cost and storage implications go in the ticket before approval: the
   archive vendor's per-GB or per-user pricing against the client's mail
   volume (mail-flow-reports has the volume numbers), growth over the
   mandated retention period, and who owns vendor-side retention/disposal.
6. Prepare execution for the tech: journal rules live in compliance/EAC
   surfaces (`New-JournalRule -Recipient <scope> -JournalEmailAddress
   <external target> -Scope <Global|Internal|External>` — verify against
   current module versions and the current portal location; this surface
   has moved between portals). Coordinate any connector needed to reach the
   archive target (email-connector-setup) with TLS enforced.
7. Verify via evidence: a test message in scope produces a journal report at
   the target (the vendor's ingestion view or the target mailbox), and an
   out-of-scope message does not. Confirm the undeliverable-report mailbox
   is set and monitored. Post a plain-text note: regulatory justification
   and approver, rule name and exact scope/direction, journal target,
   undeliverable-report mailbox and its monitor, vendor/storage cost
   acknowledgment, start date, and rollback (disable rule <name>; note that
   already-journaled copies live at the vendor under their retention).
   Log time.

## Guardrails

- No journaling without a named legal/regulatory justification and the
  client's compliance authority approving scope and duration in writing.
- Surveillance-flavored requests (a manager wanting copies of staff mail)
  are declined and documented, not quietly implemented.
- Journal target must be outside the Exchange Online organization; designs
  that journal to a local mailbox are invalid — catch it at design time.
- The undeliverable-journal-report mailbox is configured and has a named
  monitor before the rule enables; silent journaling failure is a
  compliance incident.
- Journaling copies cannot be un-copied: the rollback note states what
  remains at the vendor and under whose retention control.
