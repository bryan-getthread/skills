---
name: Post-Mortem & RCA Author
description: Write a structured post-mortem or root-cause analysis from an incident ticket — executive summary, event timeline, impact, root cause, and action items.
category: Documentation
tools: [search_tickets, search_knowledge_base, add_ticket_note]
---

# Post-Mortem & RCA Author

Assemble a blameless, evidence-based RCA/post-mortem document from the incident ticket(s),
separating confirmed fact from hypothesis at every step.

## When to use

- "Write an RCA for this outage" / "post-mortem for ticket <number>."
- A P1 closed and the client or management wants a formal write-up.
- A recurring problem finally got a root cause and it needs recording.
- Input to a client-facing PIR (pair with pir-document-generator for the branded doc).

## Steps

1. Pull the incident ticket (and any merged/related tickets) with `search_tickets`: full thread, internal notes, time entries, status changes. Ask for monitoring/vendor evidence the requester has outside the ticket rather than assuming it.
2. Build the document in this structure:
   - **Executive summary** — 3–5 sentences: what broke, who was affected, for how long, root cause in one clause, current state. Written for a non-technical reader.
   - **Timeline** — chronological table built from ticket events and timestamped notes: detection, escalations, key diagnostic findings, mitigation, resolution, client communications. Use the ticket's recorded timestamps; mark reconstructed times as "approx."
   - **Impact** — affected users/sites/services, duration, and business effect as evidenced in the thread. No invented user counts.
   - **Root cause** — the confirmed causal chain. If the true root cause is unproven, say "most probable cause" and list what would confirm it. Distinguish root cause from trigger and from contributing factors.
   - **What went well / what hindered** — response strengths and friction points, blameless phrasing (process and system focus, not people).
   - **Action items** — specific, owned, verifiable preventatives/improvements. Only include owners and dates the requester supplies; otherwise leave `Owner: TBD`.
3. Sanitize for the intended audience: internal version may name systems and staff roles; client-facing version uses <client>-safe language and no internal tooling detail. Ask which audience if unstated.
4. Output the document in chat. If asked, post the executive summary as an internal note via `add_ticket_note` linking the full doc location the requester names.

## Guardrails

- Blameless and defensive writing: never attribute fault to a named individual, and never state "breach", "hacked", or "data loss" unless the thread confirms it — use "suspicious activity" / "service disruption" until confirmed.
- Every timeline entry traces to a ticket event or note; do not fabricate timestamps, metrics, or communications.
- Keep hypothesis clearly labeled ("suspected", "unconfirmed") — an RCA that guesses is worse than one that admits an open question.
- Action items must be things the evidence supports; do not pad with generic best-practice filler.
- Draft only — publishing or sending to the client is a human decision.
