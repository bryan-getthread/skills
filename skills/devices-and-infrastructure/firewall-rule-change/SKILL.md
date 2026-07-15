---
name: Firewall Rule Change Request
description: Shepherd a firewall change request from vague ask to change-management-ready spec — business justification, exact source/destination/port/protocol, expiry for temporary rules, and routing to the approver. Use when a ticket asks to "open a port", "allow access to X", or "whitelist this vendor".
category: Devices & Infrastructure
tools: [search_tickets, search_itglue, search_hudu, search_knowledge_base, update_ticket, add_ticket_note, send_approval, create_ticket, schedule_ticket]
---

# Firewall Rule Change Request

Turn "please open a port" into a complete, reviewable firewall change: who needs it and why, the tightest rule that satisfies the need, an expiry if it is temporary, and a clean handoff into the change-management process. This skill prepares and routes the change — it never touches the firewall.

## When to use

- A ticket asks to open a port, allow inbound access, whitelist a vendor IP, or set up a port forward.
- A vendor's install guide demands firewall exceptions and the desk needs to translate it into a proper request.
- "Temporarily allow X for the audit/migration" — anything with an implied end date.
- Reviewing whether an existing rule request is specific enough to implement.

## Steps

1. Extract what is actually being asked: requester, client, business justification in one sentence, and the traffic — source (IP/subnet/"any"?), destination, port(s), protocol, direction. If the request came from a vendor document, quote the vendor's stated requirements rather than paraphrasing.
2. Tighten anything vague. "Any" as a source on an inbound rule needs explicit pushback — ask for the vendor's published IP ranges or the specific remote subnet. Broad port ranges get the same treatment: which ports does the application actually document? Never widen a request to "make it work".
3. Classify permanence: if the need is time-boxed (migration, audit, trial, contractor engagement), the request must carry an expiry date and a scheduled removal. Propose a review date even for "permanent" rules touching inbound exposure.
4. Context checks: `search_itglue` / `search_hudu` for the client's firewall documentation (which device, who administers it — the MSP, a co-managed IT team, or an ISP-managed device changes the routing entirely) and any documented change-control policy. `search_tickets` for prior requests touching the same destination — a duplicate or a previously rejected request changes the answer.
5. Assemble the change spec as a plain-text block: justification, requester, exact rule (source, destination, port, protocol, direction, NAT if applicable), permanence + expiry, rollback statement ("remove rule" — trivially reversible), risk note for anything inbound or any-source. Post it to the ticket via `add_ticket_note`.
6. Route it: send to the designated approver via `send_approval` with the spec summary. Once approved, hand off to whoever administers the firewall — via ticket assignment/`update_ticket`, or `create_ticket` on the appropriate board if the client's process requires a separate change ticket. For temporary rules, create the removal task now: `schedule_ticket` or a follow-up ticket dated at expiry, so removal does not depend on memory.

## Guardrails

- This skill never implements the rule. It produces the spec and routes it; implementation is a human action on the firewall. Never report the change as done because the spec was approved.
- No inbound any/any, and no any-source inbound rule without an explicit, named risk acceptance from the approver in writing.
- Temporary rules without a created removal task are an incomplete workflow — do not close the request until the expiry follow-up exists.
- Never include firewall admin credentials, VPN PSKs, or management URLs with embedded secrets in the ticket; reference documentation by name.
- If the firewall is not administered by this desk (ISP- or co-managed), your output is a request package for the administering party, and the ticket should reflect waiting-on-external.
- Notes and approval text are plain text — no markdown, no emojis.

## See also

- `change-request-prerequisites` (compliance-and-audit) — the generic change-control checklist this feeds into.
- `switch-vlan-change` — the LAN-side sibling of this workflow.
