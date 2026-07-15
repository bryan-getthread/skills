---
name: Easy Win Finder
description: "Recommend an easy ticket that just came in" — surface quick-win candidates from the queue that match the requesting technician's skills, with why each is quick.
category: Personal Productivity
tools: [search_tickets, search_members]
---

# Easy Win Finder

Finds the quick wins: recent tickets that look resolvable in one short touch by *this* technician, based on what they've closed before. For the fifteen minutes between meetings, the end-of-shift gap, or momentum after a brutal ticket.

## When to use

- "Recommend an easy ticket that just came in"
- "I've got 20 minutes — what can I knock out?"
- "Give me a quick win" / "something light after that last one"
- A new tech building confidence: "what's a good starter ticket in the queue right now?"

## Steps

1. Establish the candidate pool: unassigned tickets on the boards the member works (default), or their own queue if they say "from my tickets". Prefer recently arrived tickets — the "just came in" case.
2. Build the member's skill fingerprint from their own history (search_tickets on their recently closed work): the categories they close fast and clean. This is what makes a ticket easy *for them* — easy is relative.
3. Score candidates for quick-win-ness:
   - Category matches the member's fast-close fingerprint
   - Single, clearly stated issue (no multi-issue essays, no "everything is broken")
   - Known resolution pattern (same category closed in ≤1–2 touches recently on this desk)
   - Client is reachable/responsive; no pending vendor or approval dependency
   - No red flags: VIP-sensitive thread, negative sentiment, prior reopen, security-flavored content
4. Recommend the top 1–3, each with: ticket number, client (short), the ask in one line, why it's quick *for this member* ("you've closed 6 of these this month, median 12 minutes"), and the expected resolution path in a sentence.
5. Be honest when there's no easy win: say the queue has none rather than dressing up a grenade — and offer the least-bad option labeled as such ("nothing quick; lightest is #123 but the client is already frustrated").
6. If the member picks one, offer the standard pickup: assign to them and set the appropriate status (update_ticket via the normal ticket-ops path) — only on their confirmation.

## Guardrails

- Recommend only; never assign or change a ticket without explicit confirmation.
- Never disguise complexity to make a recommendation land — a mispredicted "easy win" costs trust in every future recommendation. State uncertainty when the thread is thin.
- Respect queue fairness: quick-win picking must not bypass SLA-driven priority; if something urgent outranks any easy win, say so first ("heads up: #456 breaches in an hour — that first?").
- Skill fingerprints are private coaching data for the member's own picks; do not use this skill to rank or compare technicians.
- Exclude tickets another tech is actively working, even if unassigned-looking from status alone — check for recent activity by others.
