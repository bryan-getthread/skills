---
name: Escalation Notice (Client)
description: Draft the client-facing "we've escalated this" message — one that builds confidence in the next tier without blaming the first, the vendor, or anyone else.
category: Communication
tools: [search_tickets, view_openDraft]
---

# Escalation Notice (Client)

Drafts the message that reframes escalation as progress: more expertise on the problem, not an admission the first attempt failed.

## When to use

- "This is going to L3 / the senior engineer / the vendor — let the client know."
- A ticket is being escalated and the client should hear it from us before they wonder why someone new is replying.
- The client asked "can this go to someone more senior?" and the answer is yes.

## Steps

1. Read the ticket to establish what's been tried and where the escalation is going (a senior tier, a specialist, a vendor case). The notice must be consistent with what the client already knows.
2. Draft per the Email Baseline Standard skill (communication/email-baseline-standard):
   - Frame escalation as forward motion: "To get this resolved as quickly as possible, I've brought in our <senior team / specialist team> who work on exactly this kind of issue."
   - **Continuity:** what's already been done carries over — they will not have to re-explain from scratch.
   - Who they'll hear from and when: "You'll hear from <the team / name if confirmed> by <time>" — or, if no handoff time is confirmed, when the next update will come.
   - Nothing changes for the client: same ticket, same thread, keep replying here.
3. Confidence check the draft: escalation language must not read as "we're stumped." Cut anything like "we've exhausted our options," "this has us puzzled," or "hopefully they can figure it out."
4. Present via view_openDraft.

## Guardrails

- **Never blame anyone:** not the first technician ("beyond <name>'s skill level"), not a vendor ("waiting on their slow support"), not a product, not the client's environment. Escalation notices name teams and next steps, not culprits.
- Never reveal internal tiering mechanics, SLA clocks, or staffing ("our only senior engineer is back Monday").
- Commit to a contact time only if it's confirmed; otherwise commit to an update time the desk will keep.
- If the escalation itself hasn't actually happened yet, don't announce it — a notice for an escalation that stalls internally is a broken promise. Draft only once the handoff is real (or clearly imminent per the tech).
- Degradation: view_openDraft is in-app only — over external MCP, output the draft in chat labeled "DRAFT."
