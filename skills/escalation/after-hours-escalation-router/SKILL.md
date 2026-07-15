---
name: After-Hours Escalation Router
description: Route an after-hours P1 correctly — decide whether the client's coverage terms justify a wake-up, identify who is on call, page them, and hand them a ready-to-act brief.
category: Escalation
tools: [search_tickets, search_clients, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, 'zapier: PagerDuty "Find User on Call"', 'zapier: PagerDuty "Add Trigger Event"']
---

# After-Hours Escalation Router

At 2am the question is threefold: does this client's agreement cover waking someone, who exactly gets woken, and what do they need to know in their first 60 seconds awake? This skill answers all three before anyone's phone rings.

## When to use

- An after-hours P1 arrives and the overnight dispatcher (or an unattended flow's human reviewer) asks "do we wake someone for this?"
- "Route this to on-call" outside business hours.
- An answering-service or monitoring alert claims emergency severity and needs the coverage decision made.

## Steps

1. Verify severity from the ticket itself: does the documented impact meet the desk's after-hours P1 bar (business-down, safety, security incident, data loss)? A caller saying "urgent" is a claim, not a verdict.
2. Check the client's coverage terms: search_itglue / search_hudu / search_knowledge_base for <client>'s support agreement — after-hours coverage included, business-hours-only, or billable-with-authorization. **If coverage cannot be verified in documentation, do not decide silently: page per the desk's default policy and flag the coverage question in the brief.**
3. Route by the coverage × severity matrix:
   - Covered + true P1 → wake on-call now (step 4).
   - Covered + not P1 → queue for morning; plain-text note with the reasoning; no page.
   - Not covered + true P1 → follow the desk's documented policy (typically: notify the client's authorized contact that after-hours work is billable and get authorization before dispatch); never quietly deliver unentitled emergency service, and never quietly refuse a business-down client either — escalate the decision to the duty manager if policy is unclear.
4. Identify and page: `zapier: PagerDuty "Find User on Call"` for the current on-call on the relevant schedule, then `zapier: PagerDuty "Add Trigger Event"` — lock-screen-sized summary: <client>, symptom, severity evidence, ticket reference. One page; escalate up the chain only if unacknowledged per the desk's timeout policy.
5. Prepare the on-call brief and post it as a plain-text note on the ticket before they log in: what's down and since when, impact scope, coverage status (covered / billable-authorized / pending), the caller's callback number, access notes referenced in documentation (never credentials themselves — point to where they live), and what's already been tried.
6. Update the ticket: priority reflecting the verified severity, status per the after-hours process, and a timestamped routing note recording the decision and its basis.

## Guardrails

- The routing decision must cite its basis: severity evidence + coverage source. "Woke on-call because X, per <agreement doc>."
- Coverage terms come from documentation; never assume entitlement from client size or tone. Unverifiable coverage → default policy + flag, not a guess.
- Never place credentials in the page or the note — reference the documentation location only.
- One page per incident; repeated triggers go through the escalation chain, not repeat pages to the same person.
- If PagerDuty via Zapier is not configured, fall back to the documented on-call roster in the KB and tell the dispatcher to call directly — say explicitly that no automated page was sent.
- When severity and policy genuinely conflict at the margins, wake the duty manager rather than improvising; record who decided.
