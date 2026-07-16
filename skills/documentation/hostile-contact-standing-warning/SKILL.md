---
name: Hostile Contact Standing Warning
description: Record a standing handling advisory for a difficult or hostile contact — professional, factual, behavior-not-character — and surface it when a ticket from that contact is opened or replied to. Leadership approval required to create one; mandatory review date; never client-visible.
category: Documentation
tools: [search_tickets, search_contacts, add_ticket_note, send_approval]
---

# Hostile Contact Standing Warning

Some contacts scream at L1s, threaten techs, or escalate every interaction — and every new tech walks in blind. A standing warning is a documented handling advisory attached to the contact so the desk handles them consistently and techs are protected. It is also a liability document: written wrong or leaked to the client, it becomes the incident. This skill writes it right, gates it behind leadership, and keeps it internal — always.

## When to use

- After an abusive or threatening interaction, a lead decides the desk needs a standing advisory on the contact.
- "Add a handling note for <contact> — nobody should call them without a second person on the line."
- A ticket arrives from a contact who has a standing warning, and the tech needs it surfaced before first touch or reply.
- The advisory's review date arrives and it needs re-evaluation.

## Creating a warning

1. Gather the factual basis from the record: `search_tickets` for the incident tickets — dates, ticket numbers, what was said or done (quoted or precisely described), witnesses/participants. The advisory cites incidents; it never generalizes beyond them.
2. Draft in strict behavior-not-character language:
   - YES: "On <date> (ticket #<n>), contact used profanity and threatened to 'get <tech> fired'. On <date> (ticket #<n>), contact demanded after-hours personal phone contact after being told the policy."
   - NO: personality labels ("crazy", "toxic", "a nightmare"), diagnoses, sarcasm, venting, or speculation about motives. If a sentence would embarrass the desk read aloud in court or by the client's CEO, rewrite it.
   - Then the handling guidance, stated as protective procedure: e.g. "written communication preferred; calls with a second member present; escalate to <lead role> on first sign of abuse; do not send this contact draft/informal replies."
3. Leadership approval is mandatory before the advisory exists anywhere: send the draft to the designated lead/manager via `send_approval`. Not approved (or timeout) → nothing is recorded; the draft is discarded, not parked in a note.
4. Set the mandatory review date (desk convention, e.g. 6 months out). An advisory without a review date is not valid — people and situations change, and stale warnings poison relationships silently.
5. Record the approved advisory in the desk's designated internal-only location per its convention (internal contact record field, internal documentation entry, or internal-only note tagged to the contact), including: the factual incidents, the handling guidance, approver and approval date, and the review date.

## Surfacing at intake / reply time

1. On a new ticket or before replying, check the contact (`search_contacts` + the desk's advisory location) for a standing warning.
2. If one exists, surface it to the tech as an internal plain-text note or in-chat advisory: the handling guidance first, then the citation basis. Never delay or alter the client-facing service itself — the warning changes HOW the desk engages, not WHETHER it serves them.
3. Never let advisory content leak into the reply: no tone shift that quotes it, no "per our records of your behavior". The client-facing reply reads exactly as professional as any other.

## Reviewing

1. At the review date: pull interactions since creation (`search_tickets`). Behavior resolved → recommend retiring the advisory. Continuing → recommend renewal with updated citations and a new review date. Either way, leadership decides (send_approval again); the advisory never auto-renews.

## Guardrails

- Leadership approval gates creation AND renewal — a tech's bad day never unilaterally creates a standing record about a person.
- Behavior, not character. Facts with dates and ticket numbers, or it does not go in.
- NEVER client-visible. If there is ANY uncertainty about where a note syncs or who can see a field, do not put the advisory there — on PSA-synced desks apply psa-note-visibility-rules, and when visibility cannot be guaranteed, keep the advisory out of PSA-synced notes entirely and use the desk's internal system instead. A leaked hostility advisory is a client-relationship incident and possibly a legal one.
- Mandatory review date, no exceptions; flag any advisory found without one as invalid and route to leadership.
- The advisory protects staff; it never becomes retaliation — service quality, priorities, and SLAs for the contact's tickets are unchanged.
- Genuine threats of violence or of legal action are not a documentation matter — those escalate to management immediately, outside this skill.
- Plain text only; no emphasis theatrics in the advisory (no all-caps insults, no exclamation marks).
