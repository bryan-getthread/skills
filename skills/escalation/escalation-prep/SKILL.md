---
name: Escalation Prep
description: Build a complete escalation package so a senior engineer, TAM, or third party can pick the ticket up cold, or recommend whether a ticket should escalate at all.
category: Escalation
tools: [search_tickets, add_ticket_note, update_ticket, list_boards, list_ticket_statuses, list_ticket_priorities, search_members, send_approval]
---

# Escalation Prep

Walks the technician through an escalation checklist one question at a time, then produces a structured internal note covering the situation, what has been tried, and what the next tier needs — plus the board/status/priority change, offered for confirmation.

## When to use

- "Escalate this ticket to L2/L3" or "prep this for the vendor/TAM."
- A technician asks "should this be escalated?" and wants a recommendation.
- A ticket is about to move boards and the receiving tier will pick it up cold.

## Steps

1. Read the full ticket thread. Pre-fill every checklist field you can from what is already documented: issue summary, environment (<device>, OS, app versions), impact and affected-user count, everything attempted with results, error text, and business urgency.
2. For each field still missing, ask **one question at a time** — do not dump the whole checklist as a wall of questions. Stop asking as soon as the checklist is complete.
3. Be role-aware in what you ask of whom. Ask the technician for backend facts (server state, logs, admin-console checks). Never ask an end user (<user>) to check backend systems, run admin tools, or interpret logs — for end users, only symptoms, timing, and impact.
4. Enforce the troubleshooting floor: at least one concrete troubleshooting step with its result must be documented before an escalation package is produced. If zero steps have been attempted, say so and suggest first-line steps instead of escalating.
5. Assemble the escalation note using the fixed template: Issue / Environment / Impact / Steps attempted & results / What the next tier needs to do / Why now. If a field is genuinely unknown after asking, leave it blank or mark it "not captured" — never invent contents to make the template look complete.
6. Offer the note for review — show it in full and ask before posting via add_ticket_note. Do not auto-post. On confirmation, post as plain text (no markdown or emojis, for PSA sync compatibility).
7. Confirm the target board (list_boards), status, and assignee with the technician, then apply via update_ticket. Where the pattern is recurring or scope exceeds break/fix, recommend converting to a project or change request instead of escalating.

## Guardrails

- Do not escalate on a thin note. The checklist must be complete or explicitly marked incomplete — and at least one troubleshooting step with result is mandatory.
- Never fabricate template fields, error messages, ticket numbers, or links. Blank beats invented.
- One question per turn when gathering; never ask end users about backend systems.
- The note is offered, not auto-posted; board/status/priority changes require explicit confirmation.
- Notes destined for the ticket are plain text only.

## Consolidates

Escalation requested, escalation summary/briefing, escalate-to-tier-2, interactive escalation checklist, and escalation prep skills.
