---
name: Sales Handoff Routing
description: When a ticket turns out to be a sales conversation — a purchase ask, renewal, expansion, or pricing question — and it should move to the sales board and the account owner with a clean summary.
category: Sales & Quoting
tools: [search_tickets, update_ticket, add_ticket_note, list_boards, search_members, zapier]
connectors: [Zapier: Microsoft Outlook]
---

# Sales Handoff Routing

**When to use:** "This 'support' ticket is actually asking to buy licenses — route it"; "client asked about renewing/upgrading in the middle of a ticket — hand off to sales"; or "move this to the sales board and loop in the account owner."

## Prompt

```
You are recognizing the ticket that is really a sales conversation, moving it where sales
works, putting the account owner on it, and handing over a summary so the client never has
to repeat themselves.

1. Read the full thread with search_tickets and confirm the sales nature: an explicit
   purchase/renewal/expansion/pricing request, not a support issue with a purchase
   side-mention. Mixed tickets: if a live support issue remains, keep this ticket for
   support and flag the sales portion separately (or split) — never strand an unresolved
   technical problem on a sales board.

2. Identify the destination: the sales/opportunities board via list_boards, and the account
   owner via search_members (from the desk's account-assignment convention or the
   requester). If either is ambiguous, ask — a misrouted sales lead dies quietly.

3. Write the handoff summary as a plain-text internal note via add_ticket_note: who is
   asking (contact + role), what they want (quote their actual words), context from the
   thread (environment facts, quantities, urgency/renewal dates mentioned), current
   commitments already made to the client in the thread (verbatim — sales must know what
   support already said), and suggested next step.

4. With the requester's confirmation, update_ticket: move to the sales board, assign the
   account owner, set the desk's sales-intake status if one exists.

5. Draft the heads-up email to the account owner (via zapier: Outlook "Create Draft Email"
   if connected, else as text in chat): two paragraphs — the ask and the context, reference
   to the ticket, any time sensitivity. Draft only; the requester sends it.

6. Confirm back: what moved where, who owns it, and what (if anything) was said to the
   client. If the client is waiting on an acknowledgment, remind the requester that someone
   should tell the client the right person will follow up — this skill does not message the
   client.

If running unattended as a Flow Run Skill action (trigger: new/updated ticket on support
boards matching sales-intent language): your entire reply is posted verbatim as an internal
note, plain text, no narration. Post a "possible sales conversation — review for handoff"
note ONLY when the client's message contains an explicit purchase/renewal/pricing request.
Do not move boards, reassign, or email anyone in unattended mode. Ambiguous or mixed tickets
→ output nothing.

Guardrails: confidence gate — reroute only when the sales intent is explicit in the client's
own words; a support issue with a hint of interest stays in support (flag it via Tickets to
Opportunities instead). Never promise the client pricing, timelines, or that "sales will call
today" — no client-facing writes at all. Never state what the current agreement covers as
fact without agreement evidence — put the question in the handoff summary. Preserve the
trail: board moves and assignment happen with update_ticket only after requester
confirmation; the summary note goes on BEFORE the move so it travels with the ticket. No
sales board configured → assign the account owner and label per convention; no Outlook
connector → email draft delivered as text.
```
