---
name: Field Visit Scheduler
description: Schedule an onsite visit and make the trip count — sweep for other open onsite-worthy tickets at the same site, group travel sensibly, book the visit, and draft the client confirmation.
category: Scheduling & Dispatch
tools: [search_tickets, search_clients, search_contacts, search_members, schedule_ticket, add_ticket_note]
---

# Field Visit Scheduler

Onsite time is expensive. When a ticket needs a field visit, this skill books it and consolidates: every other open ticket at that site that needs hands-on work rides along on the same trip.

## When to use

- "This needs an onsite — schedule <tech> to go out to <client>."
- "We're going to <site> Thursday, what else should we knock out while there?"
- Planning a field day: grouping visits by location.

## Steps

1. Resolve the anchor ticket, the client/site (`search_clients`, `search_contacts` for the onsite contact), and the field tech (`search_members`). Confirm the site address from the client record — never guess between multiple locations; if the client has several sites, ask which one.
2. **Consolidation sweep.** `search_tickets` for other open tickets for the same client/site and flag the onsite-worthy ones: hardware replacements, physical installs, cabling, printer/device work, anything previously deferred as "needs onsite". Present the list with ticket numbers and a rough per-ticket time estimate so the dispatcher chooses what to bundle.
3. **Travel grouping** (when scheduling multiple visits): order sites to avoid crossing back and forth, and note when another client's pending onsite sits near this one — offer to book it the same day. Present grouping as a suggestion with the reasoning; the dispatcher decides.
4. Size the visit window from the bundled work (sum of estimates + travel buffer). Check the tech's availability for the day (see Calendar-Aware Scheduling; use their calendar source if connected).
5. On confirmation, book with `schedule_ticket` on the anchor ticket for the visit window, and add a plain-text note to each bundled ticket via `add_ticket_note`: "Scheduled for onsite visit <date> with ticket <anchor>."
6. Draft the client confirmation email for the tech to send: date, arrival window, who is coming, what will be worked on (the bundled list in client-friendly terms), and what the site should have ready (access, credentials contact, parts). Do not send it yourself unless the desk has opted in.

## Guardrails

- Confirm the exact site before booking — multi-location clients are the classic wasted-trip failure.
- Bundling is opt-in per ticket: propose the ride-along list, let the dispatcher pick; never silently expand the visit scope the client wasn't told about.
- Time estimates are estimates — label them as such; don't present a packed schedule with zero slack as feasible.
- Confirm before booking and before adding notes to bundled tickets.
- Client-facing confirmation is a draft for the tech to send; plain, professional language, no internal jargon or ticket-system markup.
- Respect client-specific rules (site access hours, required escorts, named-tech-only sites) over convenience grouping.
- If the ticket search may have hit a result cap, say the ride-along list might be incomplete.
