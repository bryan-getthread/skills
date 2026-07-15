---
name: Linear Release Notes
description: Turn a completed Linear cycle's issues into client-safe release notes, then post the relevant fix notice to each affected ticket. Use for "write release notes from the last cycle", "tell clients their bug is fixed", or "close the loop on shipped escalations".
category: Connectors
tools: [list_cycles, list_issues, get_issue, list_comments, search_tickets, add_ticket_note, update_ticket]
---

# Linear Release Notes

Close the loop MSPs usually drop: engineering ships the fix, but the clients who reported it never hear. This skill converts a completed cycle into plain-language release notes and delivers the "your issue is fixed" message to every waiting ticket.

## When to use

- "Cycle just ended — draft the release notes."
- "The <symptom> bug shipped; notify all the tickets waiting on it."
- Recurring end-of-cycle communications pass.

## Steps

1. `list_cycles` for the completed cycle; `list_issues` for its issues in a done state. Exclude issues in non-shipped states (canceled, duplicate).
2. For each shipped issue, `get_issue` (and `list_comments` where the fix's user-visible effect is unclear) and translate to a **client-safe** line: what the user experienced → what is now fixed, in plain language. Strip internal jargon, code details, blame, and anything security-sensitive (a fixed vulnerability is described as "a security improvement", never with exploit detail).
3. Assemble the notes: Fixed / Improved / Known issues sections, dated. Show the draft for review — nothing external moves before the member approves the wording.
4. Map issues to waiting tickets: `search_tickets` for tickets referencing each issue id or tracked as waiting on it. Present the ticket list per issue for confirmation — this is the blast radius.
5. On confirmation, for each affected ticket: `add_ticket_note` with the relevant fix notice only (plain text, no markdown — PSA-safe; not the whole release notes), and propose the status change the desk uses for "fix shipped — verify with the client" via `update_ticket` where the member wants it.
6. Report: notes published (where the member chose to put them), tickets noted, tickets skipped and why.

## Guardrails

- **Member-authenticated connector:** requires the member's Linear connection; degrade by drafting release notes from a pasted issue list.
- Client-safe means it: no internal names, no security exploit details, no promises about future work beyond what the member approves.
- Never mark a client's issue fixed unless the Linear issue is genuinely in a done state — "in review" is not shipped. When ambiguous, leave that ticket out and say so.
- Confirm the ticket list before posting; a wrong-ticket fix notice tells a client their unfixed problem is solved.
- Ticket notes are plain text; the desk decides whether the note is internal or client-visible — ask, don't assume.
