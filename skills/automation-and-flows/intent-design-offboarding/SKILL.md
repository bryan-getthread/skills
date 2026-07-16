---
name: Offboarding Intent Design
description: Build the employee-termination intake intent with an authorized-requester check and urgency handling designed in from the start. Use when asked to "build an offboarding intent", "automate termination requests", or when user-departure tickets rank high in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets]
---

# Offboarding Intent Design

Guide the admin through building an offboarding intent that captures a complete, authorized termination request — including whether it is an immediate/for-cause lockout — and routes it to the offboarding workflow. The intent never disables anything itself; it produces a verified, dispatch-ready ticket.

## When to use

- "Build an intent for employee terminations."
- "Offboarding requests come in vague and from random people — tighten the intake."
- Intent Mining flagged user departures / disable-account requests as high volume.

## Design

**Trigger phrases (adapt to real ticket language):** "employee leaving", "offboard a user", "terminate an employee", "user's last day is", "disable an account", "someone quit", "remove a user", "employee termination", "staff member is leaving", "deactivate <user>'s account".
Near-miss watch: "remove a user from <group>" is an access change, not offboarding.

**Arguments to collect:**
- Departing user's name and the systems/accounts in scope (default: all).
- Last working day and exact cutoff time for access.
- **Urgency class:** standard (scheduled last day) vs. immediate lockout (for-cause). This single argument changes the routing.
- Requester's name and role — offboarding must come from an authorized requester (manager/HR/owner per client policy).
- Mail and data handling: forward mailbox to whom, delegate access, retain how long, device return logistics.
- Anything to preserve (litigation hold, shared credentials the team still needs — flag, never disclose).

**Reply flow:**
1. Collect arguments; if urgency = immediate, shorten intake to identity + requester + cutoff and route to the client's urgent path — do not make a lockout wait on mailbox questions.
2. **Authorized-requester check:** if the requester is not in the client's authorized-approver set (or the intent cannot tell), the reply states that offboarding requires confirmation from an authorized contact, and the ticket is flagged "authorization unconfirmed" — never silently accepted.
3. Confirm the full summary; create the ticket with a plain-text field block; route to the offboarding board/workflow.
4. Reply with next steps; no promises about when access ends — the technician confirms.

**Handoff rule:** disabling accounts, wiping devices, and credential changes are always human/workflow actions. An unverified or ambiguous termination request is a security event — escalate, do not process.

**Variation hooks (per client):** who counts as an authorized requester, the urgent-lockout routing target, mailbox retention defaults, device-return instructions, badge/physical access steps.

**Success metric:** first-touch completeness plus authorization coverage — percentage of offboarding tickets arriving with authorization confirmed and zero follow-up intake questions. Watch also the immediate-lockout time-to-dispatch.

## Steps

1. `list_intents` — check for an existing offboarding/termination intent; prefer updating.
2. `search_tickets` for recent departure tickets; mine trigger phrasing and every clarifying question techs asked (candidate arguments), and note who typically submits these.
3. Draft the full spec with the authorized-requester rule and the immediate-lockout branch explicit. Test plan: 5 should-match, 3–5 should-not (include a group-removal near-miss and a "reset password for leaving employee" case that must still hand off).
4. Show the draft; on explicit confirmation call `create_intent` then the variation tools.
5. Report what was created, restate the test plan, recommend activation after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, output the complete written spec for an admin to apply.
- The intent must never accept a termination from an unverified requester as routine — the authorization-unconfirmed flag is mandatory, and the design must not let a chat conversation alone disable a person's livelihood access.
- Never design replies that disclose the departing user's data, credentials, or mailbox contents to the requester.
- Do not invent the client's authorization policy or retention defaults; placeholder and flag before activation.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text.
