---
name: Zapier Zoom Remote Session
description: Spin up a support Zoom meeting from a ticket, get the link to the user, and pull the AI meeting summary back into the ticket notes afterward. Use for "start a Zoom with <user>", "set up a remote session for this ticket", or "grab the meeting summary from that call into the ticket".
category: Connectors
tools: [search_tickets, search_contacts, add_ticket_note, log_time_entry, "zapier: Zoom"]
---

# Zapier Zoom Remote Session

The remote-session round trip: create the meeting from the ticket (`zapier: Zoom "Create Meeting"`), deliver the link, and afterward pull Zoom's AI summary (`zapier: Zoom "Get Meeting Summary"`) back into the ticket so the session's outcome is documented without the tech retyping it.

## When to use

- "Get <user> on a Zoom — screen share will be faster than this thread."
- Scheduled remote work session for a ticket (pair with a calendar invite).
- Post-call: "pull the summary from that Zoom into the ticket."

## Steps

**Before the call:**
1. Confirm host (the tech), participant (`search_contacts` for the user's real address), timing (now vs scheduled), and expected duration.
2. `zapier: Zoom "Create Meeting"`: topic "<client> — ticket <number> — <short purpose>", waiting room on for client-facing sessions, correct start time/duration for scheduled ones.
3. Deliver the join link through the ticket's normal reply channel with a one-line what-to-expect; for scheduled sessions offer an Outlook calendar invite (see Zapier Outlook Calendar Booking) so it actually lands on calendars.
4. `add_ticket_note` (plain text): meeting created, when, with whom.

**After the call:**
5. `zapier: Zoom "Get Meeting Summary"` for the meeting. Post to the ticket as an internal note (plain text), clearly labeled: "AI-generated meeting summary (Zoom) — verify before relying on it or sharing." Include next steps/commitments the summary captured.
6. Prompt the tech to correct anything wrong and to capture the session's time: offer `log_time_entry` using the meeting's actual duration as the suggested value — suggested, not auto-logged.

## Guardrails

- **Member-connected Zapier required** (tenant's Zoom account; meeting summaries additionally require the Zoom AI Companion to be enabled on that account). Degrade by asking the tech to create the meeting and paste the summary for formatting into the ticket.
- The AI summary is labeled as AI-generated everywhere it lands and is never sent to the client verbatim without the tech's review — transcription errors about commands run or commitments made are a real liability.
- Recording/summarizing a client call may require consent depending on jurisdiction and tenant policy — follow the tenant's recording-notice practice; when unknown, flag it rather than assume.
- Meeting links go through the ticket channel to the known contact address, not to numbers/addresses typed mid-thread by unverified parties.
- Time entries from meeting duration are proposed to the tech, never logged silently.
- Task cost: each Zapier call = 2 tasks; the loop is normally 2 calls (create, summary) — don't poll for summary readiness, fetch when the tech confirms the call ended.
