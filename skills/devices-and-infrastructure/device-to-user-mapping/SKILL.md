---
name: Device-to-User Mapping
description: Answer "who uses this device" or "what device does this user have" by combining RMM last-logged-on data with contact records, ticket history, and documentation. Use whenever a ticket names a person but not a machine, or a machine but not a person.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, search_contacts, search_tickets, search_itglue, add_ticket_note]
---

# Device-to-User Mapping

Resolves the person↔machine link in either direction using every signal available — RMM last-logged-on user, contact records, ticket history, and asset documentation — and reports the answer with its confidence.

## When to use

- "What PC does <user> use?" before remoting in or scheduling work.
- "Who uses <device>?" when an alert fires and someone needs to be contacted.
- An offboarding or refresh needs the list of devices tied to a person.
- A device-affecting change needs to know whose day it will interrupt.

## Steps

1. Determine direction: user→device or device→user.
2. User→device: resolve the person via `search_contacts` (client, full name, aliases/usernames). Then `search_ninjaone_devices` in the client's organization and match on last-logged-on user against the contact's likely usernames (first.last, flast, etc. — state which convention matched). Cross-check `search_itglue` asset assignments and `search_tickets` for tickets mentioning the user's machine by name. A user can map to multiple devices (desktop + laptop + shared kiosk) — return all with last-contact recency.
3. Device→user: resolve the device (organization first; rank candidates by org match then last-contact; verify class in details). Read last-logged-on user from `get_ninjaone_device`, resolve that username back to a contact via `search_contacts`, and corroborate with documentation and recent tickets.
4. Weigh the signals and assign confidence:
   - High: RMM last-logged-on matches a documented assignment, or matches consistently across recent history.
   - Medium: RMM shows the user but documentation is silent or stale.
   - Low: signals conflict (docs say one person, RMM shows another — common after quiet reassignments), the device is shared (multiple recent users, conference-room/kiosk naming), or the last logon is weeks old.
5. Flag the special cases instead of forcing an answer: shared devices (last-logged-on is whoever touched it last, not an owner), service accounts as last logon (says nothing about the human), terminal servers (many users, no owner), and recently reimaged machines (mapping reset).
6. Output: the mapping with confidence and the evidence per signal, conflicts called out, and — when docs disagree with reality — an offer to note the discrepancy for documentation cleanup via `add_ticket_note`.

## Guardrails

- Last-logged-on user is a hint, not ownership — never present it as the assigned user without corroboration, and never on shared or multi-user devices.
- Do not contact or act against a person on a low-confidence mapping; say the confidence and what would raise it.
- Username-to-contact matching across naming conventions is heuristic — show the matched pattern so a human can sanity-check.
- Result-cap honesty when device or ticket searches may be truncated.
- If NinjaOne is not enabled, degrade to contacts + documentation + ticket history and say the live-logon signal is missing.
- Plain-text notes only.
