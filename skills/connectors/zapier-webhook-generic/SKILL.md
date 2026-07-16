---
name: Zapier Webhook Generic
description: The escape hatch — fire a generic webhook (Rewst workflow, custom automation, homegrown endpoint) from a skill or ticket when no named Zapier app covers the target system, with payload discipline, secret-free URLs, and honest response handling. Use for "trigger the Rewst flow for this", "POST this to our automation endpoint", or wiring a skill to any system reachable only by webhook.
category: Connectors
tools: [search_tickets, add_ticket_note, "zapier: Webhooks by Zapier"]
---

# Zapier Webhook Generic

When the target system has no Zapier app — a Rewst workflow, an internal automation server, a vendor's inbound hook — a raw webhook is the last-resort transport. Powerful and structureless: no field validation, no find-before-create, no vendor semantics. This skill imposes the structure the transport lacks: a documented endpoint, a disciplined payload, secret-free handling, and honesty about what a 200 response does and does not prove.

## When to use

- "Kick off our Rewst onboarding workflow from this ticket."
- A skill needs to hand data to a homegrown automation with an inbound webhook.
- "POST the alert payload to <internal system>'s endpoint."
- Bridging to any system where the webhook is the only integration surface.

## Steps

1. **Verify before promising:** confirm the Webhooks by Zapier action (POST/PUT/GET) is enabled for this tenant — on curated servers it frequently is not pinned, precisely because it can call anywhere; its absence is an admin decision to respect, not to work around via another app. Then verify the *endpoint* is a known, documented one: the member (or tenant runbook) supplies the URL, expected method, content type, and payload contract. No documented contract → stop; guessing a payload at an automation endpoint triggers automations with guessed data.
2. **Secret-free URLs and headers:** the URL must not carry credentials or long-lived secret tokens in ways that end up quoted in tickets or logs. Auth material belongs in the tenant's stored configuration (curated server locked fields, or the receiving system's own validation) — never inline in the skill's output, never pasted into a ticket note. If the endpoint's design forces a secret-bearing URL, treat the whole URL as a credential: use it, don't quote it.
3. **Compose the payload with discipline:** exactly the fields the contract specifies — stable names, ISO-8601 timestamps, the Thread ticket reference for traceability, explicit nulls over omitted-vs-empty ambiguity. Include an idempotency-friendly key (ticket number + event type) whenever the endpoint might receive retries. No payload padding: extra fields "in case they're useful" become someone's undocumented dependency. Client data in the payload is bound by the same sanitization bar as any external write — minimum necessary, no credentials.
4. Show the member the endpoint (identified by name, not secret-bearing URL), method, and full payload before firing — a webhook is an action in a system this skill cannot see into.
5. Fire the webhook and read the response honestly.
6. **Response-handling honesty — the core discipline:** a 2xx means the endpoint *accepted the request*, not that the downstream automation succeeded. Report it exactly that way: "the workflow endpoint accepted the trigger" — never "the onboarding completed". If the response body carries a job/run ID, capture it. Non-2xx or timeout → report the actual status and body (minus any secrets), do not retry blindly (the request may have been processed despite the error), and hand the member the failure verbatim.
7. Close the loop: `add_ticket_note` (plain text) — endpoint name, what was sent (summary, not secret-bearing detail), response status, any returned ID, and the honest status wording ("triggered/accepted", not "done"). If the receiving system reports completion elsewhere (a Rewst notification, a status page), name where confirmation will actually appear.

## Guardrails

- **Member-connected Zapier required**, with the Webhooks action enabled — and treat its availability as deliberate: it can call any URL, so curated-server tenants may have excluded it on purpose. Degrade: output the composed payload and endpoint name for the member to fire from their automation platform directly, and note the pending trigger on the ticket.
- Documented endpoints only: no firing at URLs improvised from memory or scraped from old tickets. The contract (URL, method, fields, expected response) comes from the member or the tenant's runbook.
- Secret-free discipline is absolute: no credentials in quoted URLs, payloads, ticket notes, or output. A webhook note that leaks a signing token converts the escape hatch into an open door.
- Never claim downstream success from transport success — "accepted" and "completed" are different facts, and conflating them is how automations fail silently for weeks.
- No blind retries on ambiguous failures; duplicate triggers into automation systems do real work twice.
- This is the escape hatch, not the default: when a named Zapier app covers the target system, use it — apps carry validation and lookups that raw webhooks lack. Reach here only when discovery has confirmed there is no app-shaped path.
- Task cost: each Zapier call = 2 Zapier tasks — a trigger is one call; webhook-per-event designs on busy boards need the same volume math as any recurring workflow, stated before wiring.
