---
name: Back-From-Vacation Catch-Up
description: A technician returning from time off — what happened on my tickets while I was out, what changed, and what needs me first, in reading order.
category: Personal Productivity
tools: [search_tickets, search_members]
---

# Back-From-Vacation Catch-Up

The re-entry briefing: everything that moved on your tickets while you were away, compressed into a triaged reading order so the first hour back is spent acting, not scrolling.

## When to use

- "I'm back from vacation — catch me up"
- "What happened on my tickets since <date>?"
- Returning from any absence: PTO, sick leave, a week of project work off the queue
- The morning-after variant: "I was out yesterday, what did I miss?"

## Steps

1. Establish the absence window. Ask for the out-from date if not given; the return date is today.
2. Pull the member's tickets and slice by what happened during the window (search_tickets; disclose result caps):
   - Tickets assigned to them with activity during the window
   - Tickets newly assigned to them while away
   - Tickets they own that were resolved/closed by someone covering
3. Build the briefing in strict needs-you-first order:
   - **Needs you now:** clients waiting on a reply, SLA at risk, anything escalated or reopened during the window. Per ticket: what changed while you were out (2–3 lines max, newest development first) and the specific next action.
   - **New to you:** tickets assigned during the absence, each with a one-line summary and current state.
   - **Handled for you:** what a covering tech resolved or progressed — name the covering tech so the member can thank them and knows who has context. One line each.
   - **No movement:** tickets exactly as they left them — count only, plus any that crossed a staleness threshold during the absence and now need a nudge.
4. Summarize the whole window in one opening paragraph before the sections: "While you were out: N tickets moved, M new, K closed by <covering tech>. The urgent one is #X."
5. For each Needs-you-now item, offer a drafted first reply that acknowledges the gap gracefully ("Thanks for your patience while I was out of office…") — drafts only, nothing sent.
6. End with a suggested first hour: the 3 actions to take before opening anything else.

## Guardrails

- Read-only by default: catching up changes nothing — no status updates, no notes, no replies — until the member acts on a draft.
- Summarize per-ticket history from what is actually in the thread; never reconstruct or guess what a covering tech "probably did".
- Keep "Handled for you" appreciative in tone — the covering tech did the member a favor; never frame their work critically in the briefing.
- If the absence window is long and the data is large, do the needs-you-now section thoroughly and honestly compress the rest ("14 more with minor activity — want them itemized?") rather than truncating silently.
- Scope strictly to the requesting member's own tickets.
