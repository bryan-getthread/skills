---
name: Vacation Handoff Pack
description: Pre-PTO handoff prep — which of my tickets need transferring, which just need watching, and a per-ticket one-liner brief so the covering tech can act without archaeology.
category: Personal Productivity
tools: [search_tickets, search_members, update_ticket, add_ticket_note]
---

# Vacation Handoff Pack

Builds the leaving-tech's handoff before PTO: triages every open ticket into transfer / watch / safe-to-sleep, and writes the covering tech a per-ticket one-liner with the context that otherwise leaves with you.

## When to use

- "I'm off next week — prep my handoff"
- "What do I need to hand over before vacation?"
- A lead preparing coverage for an absent tech
- Also works for shorter absences: "I'm out Thursday–Friday, what needs covering?"

## Steps

1. Collect: absence dates, and who is covering (a named tech via search_members, or "the queue" if coverage is pooled).
2. Pull all the member's open tickets (search_tickets) and triage each into exactly one bucket:
   - **Transfer:** will need action during the absence — active troubleshooting mid-flight, client expecting a response, SLA that will breach, scheduled work falling inside the window.
   - **Watch:** probably quiet, but could wake up — waiting on vendor/client where a reply may arrive; the covering tech acts only if it moves.
   - **Safe:** genuinely dormant (long-hold, scheduled beyond the return date). No coverage action needed.
3. For every Transfer and Watch ticket, write the one-liner brief — the sentence the covering tech actually needs, not a summary of the summary: current state, the next expected event, and the trap if there is one ("#1234 <client> — replacement switch arrives Tue, install same day; site contact is <user>, don't reboot the old one before EOD backups"). Include where the credentials/docs live by reference (e.g. "creds in the docs tool under the client's network folder") — never paste credentials into the pack.
4. Flag the specials: any VIP client, any ticket with a fragile client relationship, and anything the member knows that isn't written in the ticket — ask the member "anything in your head that isn't in these tickets?" and capture their answer into the relevant one-liners.
5. Present the full pack for review: buckets, one-liners, and the proposed transfers. Nothing has changed yet.
6. On explicit confirmation only:
   - Reassign Transfer tickets to the covering tech (update_ticket)
   - Add an internal handoff note to each transferred/watched ticket carrying its one-liner (add_ticket_note, plain text) so the context lives on the ticket, not just in chat
7. Output the final pack in a copyable format for the covering tech, ordered by urgency, with the absence dates and the returning date at the top.

## Guardrails

- No reassignment or notes without explicit confirmation of the reviewed pack.
- Never paste credentials, and never invent context: one-liners state only what the thread shows plus what the member explicitly told you.
- Watch-bucket discipline: do not transfer everything "to be safe" — burying the covering tech guarantees the real transfers get less attention. Justify each Transfer.
- Handoff notes are internal, plain text, and start with a recognizable prefix (e.g. "HANDOFF <dates>:") so they're findable and can be cleaned up on return.
- If no covering tech is identified for a Transfer-bucket ticket with a hard in-absence deadline, flag it loudly at the top of the pack — that ticket is the whole reason this skill exists.
- Pairs with Vacation Out-of-Office (client-facing messaging) and Back-From-Vacation Catch-Up (re-entry); this skill handles only the internal handoff.
