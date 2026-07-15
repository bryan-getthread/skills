---
name: Sales Handoff Routing
description: When a ticket turns out to be a sales conversation — a purchase ask, renewal, expansion, or pricing question — and it should move to the sales board and the account owner with a clean summary.
category: Sales & Quoting
tools: [search_tickets, update_ticket, add_ticket_note, list_boards, search_members, zapier]
---

# Sales Handoff Routing

Recognize the ticket that is really a sales conversation, move it where sales works, put the account owner on it, and hand over a summary so the client never has to repeat themselves.

Optional: `zapier: Outlook "Create Draft Email"` for the account-owner heads-up (member's connector required).

## When to use

- "This 'support' ticket is actually asking to buy licenses — route it."
- "Client asked about renewing/upgrading in the middle of a ticket — hand off to sales."
- "Move this to the sales board and loop in the account owner."

## Steps

1. Read the full thread with `search_tickets` and confirm the sales nature: an explicit purchase/renewal/expansion/pricing request, not a support issue with a purchase side-mention. Mixed tickets: if a live support issue remains, keep this ticket for support and flag the sales portion separately (or split) — never strand an unresolved technical problem on a sales board.
2. Identify the destination: the sales/opportunities board via `list_boards`, and the account owner via `search_members` (from the desk's account-assignment convention or the requester). If either is ambiguous, ask — a misrouted sales lead dies quietly.
3. Write the handoff summary as a plain-text internal note via `add_ticket_note`: who is asking (contact + role), what they want (quote their actual words), context from the thread (environment facts, quantities, urgency/renewal dates mentioned), current commitments already made to the client in the thread (verbatim — sales must know what support already said), and suggested next step.
4. With the requester's confirmation, `update_ticket`: move to the sales board, assign the account owner, set the desk's sales-intake status if one exists.
5. Draft the heads-up email to the account owner (via `zapier: Outlook "Create Draft Email"` if connected, else as text in chat): two paragraphs — the ask and the context, link/reference to the ticket, any time sensitivity. Draft only; the requester sends it.
6. Confirm back: what moved where, who owns it, and what (if anything) was said to the client. If the client is waiting on an acknowledgment, remind the requester that someone should tell the client the right person will follow up — this skill does not message the client.

## Guardrails

- Confidence gate: reroute only when the sales intent is explicit in the client's own words. Support issue with a hint of interest → stays in support; flag the signal via Tickets to Opportunities instead. When in doubt, do nothing and ask.
- Never promise the client pricing, timelines, or that "sales will call today" — no client-facing writes at all; the account owner owns the next client touch.
- Never state what the current agreement covers (e.g. "that renewal is already included") as fact without agreement evidence — put the question in the handoff summary instead.
- Preserve the trail: board moves and assignment happen with `update_ticket` only after requester confirmation; the summary note goes on BEFORE the move so it travels with the ticket.
- Degradation: no sales board configured → assign the account owner and label per the desk's convention; no Outlook connector → email draft delivered as text.

## Unattended (Flows) variant

- Trigger: new or updated ticket on support boards matching sales-intent language.
- Your entire reply is posted verbatim as an internal note — no narration. Plain text only.
- Post a "possible sales conversation — review for handoff" note ONLY when the client's message contains an explicit purchase/renewal/pricing request. Do not move boards, do not reassign, do not email anyone in unattended mode.
- Ambiguous or mixed tickets → output nothing. When in doubt, do nothing.
