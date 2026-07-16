---
name: Zapier Mailchimp Notices
description: Send client-broadcast notices — maintenance windows, security advisories, service changes — through the MSP's Mailchimp to the established audience/segment, never to ad-hoc lists. Use for "send the maintenance notice to all <client> contacts", "broadcast the advisory", or any one-to-many client notice bigger than a ticket reply.
category: Connectors
tools: [search_tickets, search_clients, search_contacts, add_ticket_note, "zapier: Mailchimp"]
---

# Zapier Mailchimp Notices

One-to-many client notices (maintenance windows, advisories, service changes) belong in the MSP's Mailchimp — where audiences are consented, unsubscribes are honored, and sends are logged — not in a BCC line assembled from ticket contacts. This skill drafts the notice, targets the *established* audience or segment, and gates the send behind explicit member confirmation, because a broadcast cannot be unsent.

## When to use

- "Send the maintenance-window notice for Saturday to all affected <client> contacts."
- A ticket-born advisory (vendor incident, security notice) needs to reach many client contacts at once.
- "Draft the service-change announcement in Mailchimp for review."

## Steps

1. **Verify before promising (Mailchimp's Zapier inventory is not pre-verified in this library):** run the Zapier Action Discovery pattern first — confirm what campaign/send actions actually exist and are enabled. Mailchimp's Zapier surface is historically subscriber-management-heavy (add/update subscriber, find subscriber) and campaign-send-thin; if no campaign-create/send action exists, this skill's ceiling is drafting content + confirming the audience for a human to send from Mailchimp — say that ceiling up front, not after drafting.
2. **Audience discipline — the core rule:** target only an existing audience or saved segment that the tenant maintains for this purpose (e.g. the client-contacts audience, a per-client or per-service tag/segment). Verify it exists and name it back to the member with its approximate size. **Never** build an ad-hoc list from ticket/PSA contacts for a send — those contacts never consented to marketing-channel email, and one spam complaint damages the domain every future notice depends on.
3. Compose the notice from ticket/change facts: what's happening, when (with timezone), who's affected, what the client should do, and who to contact. Client-safe, plain, no internal references. Maintenance and advisory notices are operational email — keep the tone informational, not promotional.
4. Show the member: the full notice text, the exact audience/segment name and size, and the intended send time. **Explicit confirmation before any send** — a broadcast is the least reversible write on the desk.
5. Send (or schedule) via the verified action; if only draft-creation is possible, create the draft and hand the member the final-send step explicitly.
6. Close the loop: `add_ticket_note` (plain text) on the driving ticket — notice sent/scheduled/drafted, audience name, size, and timestamp. If the notice promised a follow-up ("we'll confirm when maintenance completes"), note that obligation so the desk keeps it.

## Guardrails

- **Member-connected Zapier required** (the MSP's Mailchimp account). Degrade: produce the finished notice text and the named target audience for the member to send from Mailchimp directly, and note the pending send on the ticket.
- **Never ad-hoc lists:** no importing contacts, no building segments on the fly from ticket data, no adding subscribers as a side effect of a notice. If the right audience/segment doesn't exist, that's a setup conversation with the member — not something to improvise at send time.
- Confirmation is mandatory and specific: audience name + size + content shown before send. "Send the notice" from the member is not consent for an audience they haven't seen named.
- Respect the platform's consent model: never send to suppressed/unsubscribed contacts by any workaround, and never use Mailchimp for individual support replies — that's what the ticket channel is for.
- Content is client-facing and broad-cast: no credentials, no client-specific vulnerability details in an advisory that reaches multiple organizations, no other clients' names.
- Task cost: each Zapier call = 2 Zapier tasks — a notice should cost a handful of tasks (audience verify + create + send), not a per-recipient loop; anything shaped like per-contact calls is the wrong design.
