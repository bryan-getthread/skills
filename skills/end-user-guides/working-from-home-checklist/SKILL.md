---
name: Working From Home Checklist
description: Draft a reply-ready remote-work setup checklist for an end user — connectivity, VPN, phone, and how to get help — tailored to the client's remote stack; "send the user a work-from-home checklist."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Working From Home Checklist

Produces a client-ready sanity checklist for a user setting up (or struggling) at home — ordered so each item proves the layer below it, which turns "nothing works from home" into "item 3 fails," a ticket the desk can actually act on.

## When to use

- "Send <user> a work-from-home setup checklist."
- New-remote-arrangement tickets, storm/office-closure days, first-day-remote for a new hire.
- "User says nothing works from home" — send the checklist to localize the failure.

## Steps

1. **Verify the client's remote stack first** (search_itglue / search_hudu / search_knowledge_base / search_tickets): does remote access go through a VPN (which one), a remote-desktop/virtual-desktop portal, or straight cloud apps? Is there a softphone/telephony app remote workers need? Any client-specific remote-work policies documented (approved-device rules, hotspot allowance)? If the access model is unknown, ask the technician ONE question — the checklist's spine is the access model.
2. **Draft the checklist in dependency order**, to end-user rules — each item one check, with a what-success-looks-like cue and a note-and-continue instruction ("if this one fails, jot it down and keep going — then reply with your list"):
   - **1. Home internet works at all:** any ordinary website loads on the work laptop. (Cue: "if even this fails, the issue is your home wifi/router — restarting the router fixes most of it; your provider owns the rest.")
   - **2. Sign-in and MFA:** they can sign in to the laptop and approve the phone prompt.
   - **3. The access layer** — exactly the one this client uses: VPN connects (point at the client-specific steps per end-user-guides/vpn-connect-guide), or the remote-desktop portal loads and they can open their desktop, or cloud apps simply load. One item, their flavor.
   - **4. Mail and calendar** open and show today's mail.
   - **5. Files:** the documents they use daily open (mapped drive via VPN, or cloud files — per client stack).
   - **6. Phone/meetings:** the softphone registers or a Teams test call works (name the actual telephony app per docs; skip if the client has none).
   - **7. How to get help when your work laptop is the problem:** the desk's phone number and the portal/email reachable from a personal device — because "email the helpdesk" is useless advice when email is what's broken. Pull the correct contact channel from client docs; never invent one.
3. Add the two environment notes, brief and practical: work laptops on home wifi are fine (per docs), but public/hotel wifi has its own trap (one line pointing to the captive-portal note in end-user-guides/wifi-connect-guide); and household-bandwidth honesty ("video calls stutter when someone else is streaming — closer to the router or a cable helps").
4. Off-ramp framing at the top: "Run through these in order. Most items take under a minute. If everything passes, you're set. If anything fails, reply with the item numbers that failed — that tells us exactly where to look."
5. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- The checklist is stack-specific by construction — a generic list that mentions a VPN the client doesn't have (or omits the virtual-desktop portal they do) misdirects every failure report that comes back.
- Dependency order is the product: never alphabetize or reshuffle items; each must prove the layer beneath.
- The help-me-when-it's-broken contact channel comes from client docs — never invent a phone number or portal URL.
- Keep it to roughly 7 items — a 20-item audit becomes homework nobody finishes; extras (monitor, ergonomics, printer at home) only if the ticket asked.
- No admin steps and no home-router configuration beyond "restart it" — the desk doesn't own the user's router, and the draft shouldn't pretend it does.
- Localizable; degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled.

## Cross-references

- end-user-guides/vpn-connect-guide, end-user-guides/wifi-connect-guide — the deep-dive components items 1 and 3 point to.
- troubleshooting-playbooks/vpn-troubleshooting — tech-facing when item 3 fails and the steps were followed.
- communication/email-baseline-standard.
