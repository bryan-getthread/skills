---
name: Default Contact Autofill
description: When a ticket comes in with no contact, look up the company's documented default contact, confirm it with a search, and assign it under a confidence gate — so contactless tickets don't stall.
category: Triage & Routing
tools: [search_tickets, search_clients, search_contacts, assign_contact, add_ticket_note]
connectors: []
---

# Default Contact Autofill

**When to use:** Alert- or system-generated tickets arrive with a company but no contact, "fill in the default contact when a ticket doesn't have one", or contactless tickets fail to notify anyone or won't sync cleanly to the PSA.

## Prompt

```
Contactless tickets stall — SLAs, notifications, and PSA sync all depend on a contact. When a
ticket arrives with none, assign the company's documented default contact, but only after
confirming that contact exists and only when the company is unambiguous.

1. Read the ticket with search_tickets. If it already has a contact, do nothing.

2. Resolve the company unambiguously with search_clients. If the company itself can't be
   determined with confidence, do nothing.

3. Look up the company's documented default contact from the desk's per-company mapping
   configured alongside the skill (e.g. "this client's primary/billing contact"). Confirm that
   contact exists with search_contacts. The contact comes from the documented mapping +
   search_contacts only — never from free text in the ticket body, and never a fabricated
   contact.

4. If the company is unambiguous AND a documented default contact resolves to a real contact
   record → assign it with assign_contact. Never overwrite an existing contact — this skill only
   fills an empty one.

5. Post a plain-text note via add_ticket_note: that no contact was present and which default was
   assigned (and why).

Confidence gate: assign only when the company is unambiguous and the default contact resolves
via search_contacts. Any doubt → leave the ticket contactless and note that a default could not
be confidently assigned. Contact assignment + one note are the only writes — no status/priority/
owner changes. Notes plain text (PSA compatibility).

Unattended (Flows): fires on ticket create; the flow can scope to a contactless condition where
available. Your entire reply is the note, verbatim: "DEFAULT CONTACT ASSIGNED: <contact> for
<company>." or "NO ACTION. Reason: <contact already present | company ambiguous | no default
configured | default not found>." Deterministic stops: contact already present → no action;
company ambiguous → no action; default unconfigured or unresolvable → no action. Assign at most
once; never overwrite an existing contact.
```
