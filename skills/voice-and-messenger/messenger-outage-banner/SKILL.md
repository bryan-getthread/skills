---
name: Messenger Outage Banner
description: Draft, update, and retire the Messenger banner clients see during an incident — factual wording without cause speculation, and hard expiry discipline so stale banners don't outlive the outage. Use when an incident needs (or no longer needs) a banner.
category: Voice & Messenger
tools: [search_tickets, add_ticket_note, schedule_ticket]
---

# Messenger Outage Banner

During an incident, the banner in Messenger is the highest-leverage sentence the MSP publishes: it deflects the flood of duplicate chats — but only while it's accurate. This skill writes banner copy to the outage-notification standard and enforces the discipline of taking it down.

## When to use

- "Put up a Messenger banner about the M365 outage."
- "Update the banner — we have an ETA now."
- Incident resolved: retire the banner and close the loop.
- Banner hygiene check: "is anything stale showing in Messenger right now?"

## Steps

1. Read the incident ticket(s) via `search_tickets` for confirmed facts only: what's impacted, who's affected, current status, next-update time.
2. Draft banner copy to the same defensive-writing rules as Outage Notification, compressed for a banner:
   - One or two sentences, ~200 characters — a banner is a headline, not a bulletin.
   - State: what is affected, that the desk knows and is working on it, and what the client should do ("no need to open a ticket for this" is the whole deflection value — include it when true).
   - **No cause speculation**, no vendor blame, no "should be fixed by <time>" unless a restore time is confirmed; prefer "next update by <time>".
   - No jargon; write in the language(s) the desk's clients chat in.
3. Set the expiry plan at creation time — a banner without a removal plan is a future lie:
   - Every banner gets a review-by time (default: the next-update time, never more than a few hours out).
   - Record it on the incident ticket as a plain-text note: `BANNER LIVE: "<text>" — review/remove by <time>.` Offer `schedule_ticket` to book the review so removal has an owner.
4. Publishing: Thread's banner is configured in the Messenger/workspace admin settings — **there is no MCP tool that sets it**. Output the exact copy plus scope (all clients vs. affected clients, where the tenant supports targeting) for the admin to paste, and say precisely where it goes.
5. Updates: new confirmed facts → new banner text via the same rules; each update replaces, never stacks. Update the `BANNER LIVE` note.
6. Retirement: when the incident resolves, the banner comes down *immediately* — before the all-clear email if necessary. Optionally one short-lived "resolved" banner with its own expiry (hours, not days). Note the takedown on the ticket: `BANNER REMOVED: <time>.`

## Guardrails

- Accuracy beats reassurance: a banner that says "resolved" while users are still down does more damage than no banner.
- Expiry discipline is non-negotiable — if the review time passes with no human action, the correct escalation is a flag on the incident ticket, not silence.
- Only confirmed facts from the incident ticket go in the banner; nothing from chat rumor or unverified vendor status pages (link-free — banners shouldn't carry URLs unless the tenant's status page is the designated home).
- This skill cannot verify the banner is actually up or down; it drafts, records, and reminds. Say so rather than claiming the banner state changed.
- One incident, one banner. Multiple simultaneous incidents → prioritize the widest-impact one; a banner listing three outages reads as "everything is broken".
- Never convert the recommendation into a completed action: the note says "copy ready for admin", not "banner posted", until a human confirms.
