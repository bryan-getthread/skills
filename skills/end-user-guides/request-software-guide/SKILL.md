---
name: Request Software Guide
description: Draft reply-ready instructions telling an end user how to request new software properly — what to include so it can be approved and installed without back-and-forth — "tell the user how to request that app."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Request Software Guide

Produces a client-ready instruction block for a user who wants a new application, telling them how to request it through the client's actual process and — the real value — exactly what details to include so it can be assessed and approved without three rounds of questions.

## When to use

- "User asked us to just install X — send them how to request software properly."
- "User wants a new app; send them what we need to approve it."
- Standing guide a desk sends whenever an out-of-process "can you install this?" comes in.

## Steps

1. **Identify the client's software-request process and any approval rules first** (search_itglue / search_hudu / search_knowledge_base / search_tickets): is there a self-service portal/company app store, a required manager or IT approval, an approved-software list, or a simple ticket-to-the-desk process? Note whether the client has licensing/procurement steps or a security-review requirement for new vendors. If the process is unknown, ask the technician ONE question — sending a user to a portal that doesn't exist wastes everyone's time.
2. **Set expectations honestly.** New software often needs approval (manager, security, licensing) and isn't instant — the draft says so plainly rather than implying same-day install.
3. **Draft the instruction block** to end-user rules, one action per step:
   - The correct channel: the portal/company store if one exists (name it, cue where it is) OR "reply to this ticket / open a request with the details below."
   - **The checklist of what to include** — this is the substance of the guide:
     - Exact software name and, if known, the version or edition.
     - What they need it to do / the business reason (helps approval and helps the desk suggest an already-licensed alternative).
     - Whether it's already owned/licensed (a license key, an account, or a purchase request needed).
     - Which computer(s) it's for, and how soon it's needed.
     - Manager approval if the client requires it — name who, or that the desk will route for it.
   - The self-service note if applicable: "if it's in the Company Portal / Software Center, you can install approved apps yourself — here's how," with a cue.
   - The safety line: "Please don't download and install it yourself from the web — some installers carry unwanted extras, and work devices are managed for security. Let us get you the right, licensed copy."
   - Off-ramp: "Not sure of the exact name, or whether there's a cost? Reply with what you're trying to accomplish and we'll help you find the right tool."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Never promise approval or an install timeline** the desk can't guarantee — new software may be denied for licensing, security, or policy reasons; the draft is about how to request, not a commitment to fulfill.
- **The "don't install it yourself from the web" line stays in every draft** — it's the security point of the whole skill.
- Process match matters: sending a user to a company store they don't have teaches them to ignore the desk.
- The agent cannot itself deploy or install software (no RMM script/deploy capability) — this guide produces the user's request, and the actual install remains a tech action.
- No admin steps (packaging, deployment, license procurement) in the user block.
- Localizable; version-cautious portal cues.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- onboarding-and-access/access-request-handling — the tech-facing intake when the request lands (if present in this library).
- communication/email-baseline-standard.
