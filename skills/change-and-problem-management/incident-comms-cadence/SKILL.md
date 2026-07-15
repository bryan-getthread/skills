---
name: Incident Comms Cadence
description: During a major incident, updates go out on a clock — internal and client — even when the update is "no change." Draft each update from ticket evidence, track the next-due time, and never let the cadence silently lapse.
category: Change & Problem Management
tools: [search_tickets, add_ticket_note, search_contacts, search_clients]
---

# Incident Comms Cadence

In a major incident, silence is information — and it's always read as bad news. This skill runs the update clock set at declaration: who gets updated, at what interval, saying what, and it treats a missed update as an incident-response failure in its own right.

## When to use

- A major incident was declared (major-incident-declaration set the cadence) and updates need drafting on schedule.
- "Draft the next client update" / "what do we tell them at the top of the hour?"
- The comms lead wants a check: are we current, and when is the next update due?
- Post-incident: reconstructing what was communicated when (feeds rfo-letter and the post-mortem).

## Steps

1. Load the cadence from the master incident ticket: intervals (default internal 30 min / client 60 min; a client's contractual terms override when stricter), audiences, and the comms lead who approves sends.
2. **Who says what** — two different artifacts, never one message dual-purposed:
   - **Internal update** (posted as a plain-text note on the master ticket, visible to the response team): technical state, workstream progress, hypotheses including uncertain ones, resource needs. Candid.
   - **Client update** (drafted for the comms lead): impact in the client's terms, what's being done stated plainly, next update time, and an ETA ONLY if the IC has committed one. Follows the first-notification framing from outage-notification (communication); anything security-flavored follows the defensive-writing-standard (security) — no "breach," no cause speculation.
3. Draft each update strictly from ticket evidence since the last update. The no-progress update is legitimate and has a fixed shape: "still working, here's what we've ruled out, next update at <time>." Never invent progress to make an update feel worthwhile.
4. Every client update states when the next one comes — and that promise becomes the new deadline. An update that goes out late is still sent, with the honest timestamp; the lapse is noted internally, not papered over.
5. Route client-facing drafts to the comms lead for approval and send; confirm the audience list against the client's documented incident contacts (search_contacts) — updates to the wrong contact list are a common majors failure. Record each sent update (time, audience, gist) in a note so the comms trail is reconstructable.
6. On stand-down: draft the closure notice (service restored, monitoring continues, RFO to follow — see rfo-letter) and record cadence end.

## Steps (cadence check variant)

1. Read the master ticket's comms notes, compute last-sent vs. interval for each audience, and report: current / due in N minutes / OVERDUE by N minutes, with the next drafts ready.

## Guardrails

- The agent drafts and tracks; the comms lead (a human) approves and sends anything client-facing. No unattended client sends during a major.
- Never state a cause to clients before the IC confirms it — early cause statements are almost always wrong and live forever in inboxes.
- Never promise an ETA the IC hasn't committed to; "next update at <time>" is the only promise the drafter may originate.
- One clock, one owner: if the comms-lead role is unassigned or ambiguous, flag it to the IC immediately — cadence without an owner is how updates stop.
- Internal candor, external discipline: hypotheses stay internal until confirmed; client updates contain only what the desk is prepared to stand behind in writing.
