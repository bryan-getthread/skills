---
name: Exchange On-Prem Mail Flow
description: Diagnose on-premises Exchange transport problems — messages stuck in queues, send/receive connector faults, TLS/cert failures, backpressure and disk-full transport stalls — from the queue viewer and protocol logs, not guesswork. For the hybrid seam use exchange-hybrid-issues.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Exchange On-Prem Mail Flow

On-prem Exchange transport breaks in known places: a queue backing up behind one destination, a connector misconfigured or pointing at the wrong smart host, a TLS/certificate failure, or the transport service throttling itself under backpressure (usually disk). This playbook reads the queues and logs first. Delivery problems that cross a hybrid boundary belong to exchange-hybrid-issues; general mail-delivery diagnosis to mail-flow-delivery.

## When to use

- Mail queuing on-prem and not delivering, internally or externally
- A send/receive connector broke after a change; "smart host" or relay stopped working
- TLS/certificate errors in mail flow, or a partner stopped accepting mail
- Transport service stopped, backpressure warnings, or the Exchange disk filled up

## Steps

1. **Version identification first.** search_itglue / search_hudu / search_knowledge_base for the design: Exchange version and CU (an out-of-support CU changes what's advisable and safe — note it), single vs multi-role, the send-connector topology (direct-to-internet vs smart host), what handles inbound (a filtering gateway/MX in front?), and the transport certificate in use. If this org is hybrid, stop and use exchange-hybrid-issues for anything crossing the boundary.
2. **History first.** search_tickets for this client + mail: a recent certificate renewal (transport TLS dies on cert expiry — sudden onset on a date is a cert until proven otherwise), a connector edit, an IP/firewall change, or a filtering-vendor change. Certificate and connector edits are the usual triggers.
3. **Read the queues and logs before theorizing.** Queue Viewer / `Get-Queue` — which queue, how deep, and the last error per queue (this alone usually names the destination and cause). Then the relevant protocol/transport logs: SMTP send/receive logs for a connector fault, and verbatim NDRs (the enhanced status code and generating server say which hop rejected). Read the actual error — not "mail isn't flowing".
4. **Branch:**
   1. **Queue backing up behind one destination** — one remote domain's queue is deep with a retry error: the far side is down/greylisting, your sending IP is on a blocklist (check reputation), or DNS/MX resolution for that domain is failing from the server. A single stuck message poisoning a queue can be handled per Exchange's tools; a reputation/blocklist problem is a delisting process, said honestly. Escalate when: the block is your public IP's reputation — that's a deliverability/DNS effort (pair with dmarc-spf-dkim-setup), not a connector tweak.
   2. **Send/receive connector fault** — a change broke relay or outbound: an anonymous relay receive connector's permitted-IP scope edited, a send connector's smart host/address-space/credentials wrong, or authentication changed. Read the connector config against the documented design; a "cleaned-up" connector is a classic cause. Never widen an anonymous relay connector's scope to "make it work" — open relays get abused.
   3. **TLS / certificate failure** — mail fails with TLS errors after a cert renewal or expiry. The transport certificate must be enabled for SMTP and referenced by the connector; a renewed cert with a new thumbprint often isn't re-bound. Verify the cert is valid, trusted by partners, and assigned to SMTP. Pair with ssl-certificate-renewal.
   4. **Backpressure / transport stalled** — the Transport service is rejecting/deferring because a resource crossed a threshold, almost always the disk holding the queue database or logs filling up (runaway logging, a huge queue, or logs never truncated). Free space at the source (why did it fill?) rather than just deleting; a stopped transport service that won't start cleanly needs the event log read. Escalate when: it's a storage-sizing problem.
5. **Verify and note.** Success is the queue draining and a test message each direction with clean headers/NDR-free delivery. Plain-text note: version/CU, the queue/last-error evidence, branch, action taken or handed off, verification and time.

## Guardrails

- No remote execution — all EMS/EAC and queue operations are guidance for a tech with Exchange admin access; use get_ninjaone_device_link to reach the server when the RMM integration is enabled.
- Never open or widen an anonymous relay connector to resolve a relay complaint — an open relay is a security incident waiting to happen; scope it to known hosts and escalate the design if needed.
- Don't mass-delete queued messages to "clear" a queue — messages are mail; suspend/inspect, and when removal is warranted export first. Removing NDR/poison messages is targeted, not wholesale.
- On out-of-support CUs, say so and set expectations honestly — some fixes require patching first.
- Quote NDR/enhanced status codes verbatim and verify meanings against Microsoft's current docs (web_search / Microsoft Learn) — do not paraphrase from memory.
- Docs/Liongard coverage varies per tenant — note what you couldn't check. Notes destined for a PSA sync: plain text, no markdown or emojis.
