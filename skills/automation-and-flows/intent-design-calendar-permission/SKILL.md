---
name: Calendar Permission Intent Design
description: Build the calendar share/delegate intent — capture whose calendar, whose access, the permission level, and the owner's approval so the ticket arrives ready to action. Use when asked to "build a calendar sharing intent", "automate calendar delegate requests", or when calendar-access ranks high in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets]
---

# Calendar Permission Intent Design

Guide the admin through building an intent that captures a clean calendar-sharing or delegation request — whose calendar, who gets access, at what level, and who approved it. The intent NEVER changes calendar permissions itself; it captures and routes with the calendar owner's approval attached.

## When to use

- "Build an intent for calendar sharing / delegate access requests."
- "Calendar-access tickets never say what permission level is needed — fix the intake."
- Intent Mining flagged calendar delegation as a recurring request.

## Design

**Trigger phrases (adapt to real ticket language):** "share my calendar", "give me access to their calendar", "add me as a delegate", "can't see my manager's calendar", "calendar delegate access", "let my assistant book my meetings", "grant editor access to my calendar", "view someone's calendar".
Near-miss watch: "book a meeting room" belongs to the meeting-room intent; "shared mailbox" belongs to the shared-mailbox intent — keep triggers scoped to *calendar* access.

**Arguments to collect:**
- Whose calendar (the owner — the resource).
- Who should receive access (the grantee — self or named others).
- Permission level: free/busy, read, editor, or full delegate (delegate can accept/decline invites on the owner's behalf).
- Owner's approval — required whenever the requester is not the calendar owner.

**Reply flow:**
1. Collect the arguments; confirm owner, grantee, and permission level back to the requester.
2. If approved (requester owns the calendar, or the owner has signed off) → create the ticket with a structured plain-text block and route to the access/M365 workflow.
3. If the requester is asking for someone else's calendar without owner sign-off → ask for the owner's approval before creating the ticket.

**Handoff rule (non-negotiable):** the intent never changes calendar permissions — delegation is always an approved human/workflow action. State that access follows owner approval.

**Variation hooks (per client):** which permission levels the client offers, whether managers auto-approve delegate access for direct reports, the owner-approval routing, which board or flow receives the ticket.

**Success metric:** first-touch completeness — percentage of calendar-permission tickets arriving with owner, grantee, level, and approval captured (zero follow-up questions).

## Steps

1. `list_intents` — check for an existing calendar/access intent; prefer `update_intent` over a duplicate.
2. `search_tickets` for recent calendar-access tickets; mine phrasing (triggers) and the follow-ups techs asked (usually the exact permission level and the owner's approval → arguments).
3. Draft the full spec: triggers, arguments, confirmation reply, routing target, variations, and a test plan (5 should-match, 3–5 should-not including a room-booking near-miss). Show before any write.
4. On explicit confirmation: `create_intent`, then `set_variation_arguments` / `set_variation_replies` / `update_variation`.
5. Report what was created, restate the test plan, recommend activation only after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, degrade to a complete written spec for an admin to apply.
- The intent captures and routes — it NEVER changes permissions. Delegation follows calendar-owner approval through the client's process.
- Require owner approval whenever the requester is not the calendar owner.
- Do not invent permission levels the client doesn't offer or an approval policy you can't confirm — placeholder anything unknown and flag it before activation.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text (PSA-safe, no markdown).
