---
name: Client-Facing Device Report
description: Produce a sanitized device inventory and health report a client contact can read — counts, health summary, and risks in business language, no internal jargon, no raw tool output. Use for "send <client> a summary of their devices" or QBR fleet slides.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, list_ninjaone_alerts, add_ticket_note]
---

# Client-Facing Device Report

Builds the fleet story for the client's eyes: how many devices, how healthy, what risks deserve budget or attention — written for a business reader, stripped of RMM jargon, alert codes, and anything internal.

## When to use

- "Put together a device report I can send to <client>."
- QBR / vCIO prep needs the fleet section in client-ready form.
- A client asks "what do you actually manage for us?"

## Steps

1. Resolve the client organization and pull the fleet (`search_ninjaone_devices`, details via `get_ninjaone_device`, active alerts via `list_ninjaone_alerts`). Verify device classes in details. If listings hit caps, get full coverage by segment before writing — a client-facing count must not be a truncated one; if full coverage is impossible, present ranges honestly ("more than N").
2. Compute the client-safe aggregates: total managed devices split by type (computers, laptops, servers); currently healthy vs needing attention; devices nearing replacement age; operating systems approaching end of support.
3. Translate everything into business language. No: hostnames of internal infrastructure beyond what the client knows, alert type codes, RMM/tool names, node classes, internal tech names, ticket numbers, or raw metrics. Yes: "3 computers are running low on storage and may slow down", "2 machines run a Windows version that stops receiving security updates in <month/year>", "1 server has needed multiple restarts recently and we are investigating."
4. Frame risks as recommendations with a why, not alarms: each risk gets one plain sentence of impact and one of what you propose. Never imply negligence, never speculate about security incidents, and never promise outcomes or prices.
5. Structure the report: one-paragraph summary (fleet size, overall health statement), a small counts table, "What needs attention" (3–5 items max, ranked), "Planning ahead" (aging hardware / OS end-of-support), and "What we're doing" next steps. Keep it to one page of reading.
6. Show the draft to the requester for review before it goes anywhere — this skill produces the draft; sending it to the client is the human's call. Offer to attach it to the ticket as a plain-text note via `add_ticket_note`.

## Guardrails

- Nothing internal leaks: no tool names, alert codes, internal hostnames, tech names, or ticket references in the client-visible body. Read the draft once as the client before delivering it.
- Numbers must be exact or honestly ranged — a wrong device count in front of a client damages trust and billing conversations.
- No security claims either direction: neither "you are fully secure" nor unconfirmed incident language. Defensive writing throughout.
- No pricing, quotes, or committed dates — planning items point to a follow-up conversation.
- The requester reviews before any client delivery; this skill never sends anything itself.
- Localizable: write in the client's language if the requester says so; avoid idioms.
