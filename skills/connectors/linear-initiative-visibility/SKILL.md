---
name: Linear Initiative Visibility
description: Read Linear projects and initiatives to answer "when will X ship?" client questions honestly — current status, target dates as recorded, and explicit uncertainty, never invented commitments. Use for "what do I tell <client> about the <feature> timeline", "status of the project fixing their issue", or roadmap-visibility asks against Linear.
category: Connectors
tools: [search_tickets, add_ticket_note, list_projects, get_issue, list_issues, list_comments, get_document, list_documents]
---

# Linear Initiative Visibility

Clients ask the desk "when is that fixed/shipping?" and the worst answers are a guess or a promise. This skill reads what Linear actually records — project status, target dates, health, the linked issue's state — and turns it into an honest, client-safe answer: what's targeted, what's moving, and what nobody has committed to. The desk relays engineering's recorded intent; it never authors a roadmap.

## When to use

- "<client> is asking when the <feature> lands — what's the real status?"
- A ticket is waiting on an escalated Linear issue (see Engineering Escalation to Linear) and the client wants a timeline.
- QBR prep: "which in-flight projects should this client hear about?"

## Steps

1. Locate the work: `list_projects` and find the relevant project by name/scope; for a specific escalated defect, `get_issue` on the known issue ID (from the ticket's escalation note) and read its project/state. Initiative-level tools shipped in Linear's Feb 2026 MCP update, but their names are unverified — try them only if present in the live tool list, and **degrade to `list_projects`** (initiatives are containers of projects; project-level status usually answers the client's question anyway).
2. Read what's recorded, not what's hoped: project status/health, target date *as entered*, recent movement (issue states via `list_issues`, discussion via `list_comments`, project docs via `get_document` where they exist). Note staleness — a target date untouched for months is a weaker fact than one updated last week, and the answer should reflect that.
3. Translate to client-safe register, preserving epistemic honesty:
   - Recorded target date → "currently **targeted** for <timeframe>" — targeted, never "will ship".
   - No target date → say so: "in progress, no committed date" beats a soothing invention.
   - Status/health signals slippage → don't hide it; "the target moved" is survivable, a blown secret promise is not.
   - Strip engineering-internal detail (names, internal debates, other clients' mentions) — status and timeframe only.
4. Deliver the answer with provenance in chat ("per Linear as of today"), and when a ticket drove the ask, `add_ticket_note` (plain text): what was checked, the recorded status/target, and what was relayed to the client.
5. If the client needs notification when it actually ships, wire that expectation into the escalation loop (Linear Fix-Shipped Loop) rather than promising to remember.

## Guardrails

- Requires member-connected Linear; if the tools are absent, degrade to answering from the ticket's last escalation note with its date, clearly labeled as possibly stale — never fabricate a status.
- Read-only skill: no issue/project writes; the desk observes the roadmap, it doesn't edit it. (A comment on an escalated issue asking for status is the one exception, and only on the member's request.)
- Targeted ≠ committed, and the client-facing words must keep that distinction; nothing this skill outputs converts an engineering target into a contractual date.
- Initiative tool names are unverified — probe gently, fall back to `list_projects` without drama, and never claim initiative-level data that wasn't actually read.
- No internal Linear content (names, comment threads, other clients) leaks into client-facing text; the answer is status + timeframe + caveat.
- Anything relayed to the client gets recorded on the ticket — the desk must be able to reconstruct exactly what timeline was communicated, and when.
