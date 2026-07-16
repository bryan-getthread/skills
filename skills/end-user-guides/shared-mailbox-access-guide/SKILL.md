---
name: Shared Mailbox Access Guide
description: Draft reply-ready instructions for an end user to open a shared mailbox they've been granted — desktop, web, and mobile paths — "tell the user how to open the shared mailbox."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Shared Mailbox Access Guide

Produces a client-ready instruction block for the moment after access is granted: how the shared mailbox actually shows up, on which device, and how long that takes — because "you now have access" without the how is just a slower ticket.

## When to use

- Access-request tickets at the fulfillment step: "access granted — send <user> the steps to open it."
- "User says they were given access to <mailbox> but can't see it."
- Role changes where a team mailbox comes with the job.

## Steps

1. **Verify access state and environment first.** From the ticket (search_tickets): has the grant actually been completed, and to which mailbox (use its display name only)? From client docs (search_itglue / search_hudu / search_knowledge_base): which Outlook flavors the user runs (classic Windows, new Outlook, Mac, web, mobile) and whether the user also has send-as/send-on-behalf rights — that changes what you promise. If the grant isn't confirmed done, do NOT send this guide; tell the tech the fulfillment step is still open. If the flavor is unknown, cover web (works everywhere) plus ONE desktop flavor confirmed from the ticket — not all five.
2. **Draft the matching paths**, to end-user rules, one action per step with what-you'll-see cues:
   - **The patience preamble** (this kills most callbacks): "In desktop Outlook the new mailbox usually appears by itself in the folder list on the left, under your own folders — but it can take up to a few hours after access is granted. If it's not there yet, that's normal."
   - **Web path (the immediate gratification route):** sign in to webmail, right-click their own name in the folder list, choose the add/open shared folder option (cue: "a box asks for a name — type <mailbox display name> and pick it from the list"), and where it then appears.
   - **Desktop path:** mostly "wait for it to appear, then restart Outlook once if it hasn't" — include manual-add steps only for the confirmed flavor, phrased version-cautiously.
   - **Sending as the mailbox** (only if the grant includes it): the From-field cue ("when writing an email, look for the From button; choose <mailbox display name> so the reply comes from the team address, not your own"), and the first-time note that the From button may need to be enabled from the options menu.
   - Off-ramps: "If it hasn't appeared anywhere by tomorrow morning, reply here." / "If you can read the mailbox but sending is rejected, stop and reply — that's a separate permission on our side."
3. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Never send this guide before the grant is confirmed complete** — instructions for access that doesn't exist yet manufacture a "your steps don't work" reply.
- Promise send-as only when the ticket/grant confirms it; reading and sending are separate permissions and conflating them is the #1 follow-up ticket.
- The propagation-delay expectation appears in every draft — it's the difference between a satisfied user and an it's-broken callback in ten minutes.
- No admin steps (adding members, mailbox permissions, automapping settings) in the user block.
- Refer to the mailbox by display name only; no addresses of uninvolved mailboxes, no permission-holder lists.
- Localizable; degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled.

## Cross-references

- troubleshooting-playbooks/outlook-client-issues — tech-facing when the mailbox never appears.
- end-user-guides/out-of-office-guide — the companion ask on shared mailboxes (OOO on a shared box is admin-side; that guide's off-ramp covers it).
- communication/email-baseline-standard.
