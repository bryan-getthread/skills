---
name: Client RCA Summary
description: Draft a client-safe root-cause summary for a resolved issue — what happened, impact, cause, and what prevents recurrence — written defensively. Use for a single-ticket RCA the client asks for, short of a full major-incident letter.
category: Communication
tools: [search_tickets, search_knowledge_base, view_openDraft]
---

# Client RCA Summary

Draft a concise, client-facing root-cause summary for a resolved issue: the facts of what happened, the impact, the confirmed cause, and the concrete steps that reduce the chance of recurrence. Written so every sentence holds up if the email is forwarded to the client's leadership or their insurer.

## When to use

- "The client wants to know why this happened — draft an RCA."
- "Write a root-cause summary for this resolved ticket."
- A single incident is resolved and the client asks for an explanation short of a formal outage letter.

## Steps

1. Read the full ticket history — timeline, actions taken, resolution — before writing. Every claim in the summary must trace to the record.
2. Structure the summary:
   - **What happened** — a plain, factual description of the issue and when it occurred.
   - **Impact** — who/what was affected and for how long, as recorded.
   - **Root cause** — the confirmed cause. If the cause is still uncertain, say "our current assessment is…" and do not present a hypothesis as fact.
   - **Resolution** — what fixed it.
   - **Prevention** — the specific steps taken or planned to reduce recurrence. Only list steps that are real; don't manufacture a prevention plan to sound thorough.
3. Keep it tight and non-technical; gloss any unavoidable technical term.
4. Apply defensive writing per the Email Baseline Standard: state facts, not speculation; never call an unconfirmed event a "breach" or "compromise"; never assign blame to a vendor, the client, or a prior tech unless it is confirmed and appropriate to share.
5. Present the draft with view_openDraft for review. Do not send.

## Guardrails

- **Defensive by default.** Write assuming a lawyer will read it. No admission of fault, no speculation stated as fact, no naming a third party's failure that isn't confirmed.
- **Cause honesty.** If root cause is not definitively known, label it as an assessment and say investigation continues — never fabricate certainty.
- **Prevention must be real.** Only cite preventive measures actually taken or committed by the record.
- Distinct from documentation/post-mortem-rca-author (the internal, technical post-mortem) — this is the client-safe version. For a major-incident outage letter use change-and-problem-management/rfo-letter.
- Degradation: view_openDraft is in-app only. Over external MCP, output the draft in chat labeled "DRAFT — review before sending."
