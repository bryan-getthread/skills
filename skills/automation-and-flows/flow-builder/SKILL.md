---
name: Flow Builder
description: Design and create an automation flow from a plain-English ask — trigger, filters, actions, notification channels — with a dry-run description before anything is created. Use when asked to "build a flow", "automate X when Y happens", "notify the channel when a P1 comes in", or "set up an automation".
category: Automation & Flows
tools: [list_flows, get_flow, create_flow, update_flow, list_flow_actions, list_flow_filter_attributes, list_flow_filter_attribute_values, list_boards, list_ticket_statuses, list_ticket_priorities]
---

# Flow Builder

Turn "when a ticket does X, do Y" into a correctly filtered flow — mapped against the tenant's real attributes and actions, described as a dry run first, and never activated without the admin's say-so.

## When to use

- "When a P1 comes in on the <client> board, notify the escalations channel."
- "Auto-set new email tickets to Needs Triage and assign the dispatcher."
- "Build a flow that runs the closure-QA skill when a ticket closes."

## Steps

1. Parse the ask into trigger event, filter conditions, and actions. Ask one round of clarifying questions if the trigger ("created" vs "updated" vs "status changed to") is ambiguous — this is the most common flow bug.
2. Map every condition to real attributes: `list_flow_filter_attributes` for the attribute, `list_flow_filter_attribute_values` for its exact values. Never guess a value string — "Priority 1" and "P1 - Critical" are different tenants' realities.
3. Map every action via `list_flow_actions`; use `list_boards` / `list_ticket_statuses` / `list_ticket_priorities` for referenced ids.
4. Check `list_flows` for existing flows on the same trigger and board. Flag overlaps: two flows firing on the same event can conflict or double-notify, and flow ordering may swallow the new one.
5. Present a **dry-run description** before creating anything: "This flow fires when <event>. It matches tickets where <filters>. It then <actions>." Walk one concrete example ticket through it, and one that would NOT match. For notification actions, state the expected message frequency ("roughly N/day based on recent volume") so nobody builds a channel-spammer.
6. On explicit confirmation only: `create_flow` (or `update_flow`) in an inactive state where supported.
7. Report what was created, restate the dry run, and tell the admin to activate it themselves after eyeballing it — never activate without a separate, explicit confirmation.

## Guardrails

- Flow tools are admin-only; if absent, output the full flow spec (trigger, filters with exact values, actions) for an admin to apply.
- Never `create_flow` before the dry-run description is confirmed. Never activate without explicit confirmation.
- Never invent filter values — everything comes from `list_flow_filter_attribute_values`.
- If a flow embeds an unattended agent prompt, apply the Unattended Output Discipline base skill to that prompt.
- Estimate blast radius before notification/auto-reply actions; a mis-filtered flow on a high-volume board is a client-facing incident.
