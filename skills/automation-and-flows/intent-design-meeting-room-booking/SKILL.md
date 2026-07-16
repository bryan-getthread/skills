---
name: Meeting Room Booking Intent Design
description: Build the meeting-room/resource booking intent — a router that points users to the client's booking system and only makes a ticket when self-service can't. Use when asked to "build a room booking intent", "handle resource booking requests", or when room-booking questions show up in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets, search_knowledge_base]
---

# Meeting Room Booking Intent Design

Guide the admin through building a router intent for meeting-room and bookable-resource requests. Booking a room is self-service in the calendar system — the intent's job is to route the user to the right booking path, and only create a ticket for the genuine IT problems (a room resource that's broken, missing, or needs to be created).

## When to use

- "Build an intent for meeting room bookings."
- "People ask the helpdesk to book rooms — send them to the right place."
- Intent Mining flagged room/resource booking questions.

## Design

**Trigger phrases (adapt to real ticket language):** "book a meeting room", "reserve the conference room", "how do I book a room", "the room isn't showing in Outlook", "can't book the boardroom", "add a room to my meeting", "reserve equipment for a meeting", "room resource missing".
Near-miss watch: "calendar sharing/delegate" belongs to the calendar-permission intent; "AV/projector broken in the room" is a hardware/troubleshooting issue — route those out.

**Arguments to collect (only enough to route correctly):**
- Is this a how-do-I-book question, or a problem with a room resource (can't find it, won't accept the booking, needs creating)?
- Which room/resource and which system (Outlook/M365 room mailbox, or a dedicated booking tool).
- For a problem path only: what happens when they try, and when they need the room.

**Reply flow (route-first):**
1. If it's a how-to and the booking is self-service → reply with the client's booking steps or KB link (add the room in Outlook, use the booking tool) and confirm that resolves it. This is a deflection.
2. If the room resource is broken/missing/won't accept bookings, or a new bookable resource is needed → create a ticket with the resource, the system, and the symptom, and route to the M365/resource-admin workflow.
3. If it's actually an AV/hardware problem in the room → route to the appropriate hardware/troubleshooting path, not this one.

**Handoff rule:** creating or fixing room mailboxes / resource calendars is an admin action. The intent routes and, where self-service exists, deflects — it does not book rooms on the user's behalf.

**Variation hooks (per client):** which booking system the client uses, the self-service KB link, whether room creation is on request, the resource-admin routing target.

**Success metric:** routing accuracy — percentage of conversations sent down the correct path (self-service deflection vs. resource-admin ticket vs. hardware). Secondary: deflection rate on the pure how-to-book path.

## Steps

1. `list_intents` — check for an existing booking/resource intent; prefer `update_intent` over a duplicate.
2. `search_tickets` for past room-booking contacts; separate the how-to questions from the genuine resource problems — that split defines the router branches.
3. `search_knowledge_base` for the client's room-booking guide; reference it in the self-service reply rather than restating steps.
4. Draft the full spec: triggers, the routing arguments, the branch replies, routing targets, variations, and a test plan (5 should-match across branches, 3–5 should-not including a calendar-delegate and an AV-hardware near-miss). Show before any write.
5. On explicit confirmation: `create_intent`, then `set_variation_arguments` / `set_variation_replies` / `update_variation`.
6. Report what was created, restate the test plan, recommend activation only after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, degrade to a complete written spec for an admin to apply.
- Router discipline: the biggest failure mode is mislabeling a hardware/AV problem or a calendar-delegate request as a booking question — test those near-misses explicitly.
- The intent does not book rooms; where booking is self-service it deflects with the client's steps, otherwise it routes to resource-admin.
- Do not invent the client's booking tool or KB link — placeholder anything unknown and flag it before activation.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text (PSA-safe, no markdown).
