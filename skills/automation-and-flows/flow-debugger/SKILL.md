---
name: Flow Debugger
description: Diagnose why a flow or intent didn't fire on a ticket — filters vs actual attributes, flow ordering, board scoping, trigger-event mismatch. Use when asked "why didn't my flow run", "the intent didn't trigger", or "this automation isn't working".
category: Automation & Flows
tools: [list_flows, get_flow, list_intents, get_intent, search_tickets, list_flow_filter_attributes, list_flow_filter_attribute_values]
---

# Flow Debugger

Compare an automation's configuration against the ticket it should have caught, filter by filter, and name the exact mismatch — then propose the minimal fix.

## When to use

- "Ticket <number> should have triggered the P1 flow — why didn't it?"
- "My intent never fires even when users type the trigger phrase."
- "This flow works on one board but not another."

## Steps

1. Fetch both sides: `get_flow` (or `get_intent`) for the configuration, `search_tickets` for the ticket's actual attributes at the time of the event.
2. Check the cheap causes first, in order:
   - **Active?** An inactive flow fires on nothing.
   - **Trigger event** — "created" flows never see tickets that arrived matching and were later updated into scope; "status changed to X" only fires on the transition, not on tickets already sitting in X.
   - **Board scoping** — is the ticket's board in the flow's scope?
3. Then walk every filter in a pass/fail table: filter, expected value, ticket's actual value, verdict. Use `list_flow_filter_attribute_values` to catch value-string mismatches ("High" vs "P2 - High").
4. Check **ordering and competition**: `list_flows` for other flows on the same trigger — an earlier flow may have changed the attribute (e.g., moved the status) before this one evaluated, or already claimed the ticket.
5. For intents: compare the user's actual message text against the trigger phrases; check whether another intent's phrases matched first; check whether the message arrived on a channel intents don't cover.
6. Output: the single named root cause (or ranked candidates if genuinely ambiguous), the pass/fail table as evidence, and the minimal proposed fix. Apply the fix via `update_flow`/`update_intent` only after explicit confirmation.

## Guardrails

- Diagnose first, change second — never edit a flow or intent as part of the investigation.
- Attribute values may lag after a recent update elsewhere; re-fetch the ticket before trusting a surprising value.
- If the ticket data genuinely matches every filter and the flow was active, say so plainly and recommend the member raise it with Thread support — do not invent an explanation.
- Flow/intent tools are admin-only; without them, ask the member to paste the flow's configuration and debug from that.
