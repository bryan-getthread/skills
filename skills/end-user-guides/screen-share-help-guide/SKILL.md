---
name: Screen Share Help Guide
description: Draft reply-ready instructions for an end user to start a remote-support screen share with the desk using the client's actual tool — "send the user how to let us connect to their screen."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Screen Share Help Guide

Produces a client-ready instruction block for a user who needs to let a technician see or control their screen for support, written for the specific remote-support tool the client uses — the join flow differs completely between tools, and the user is often already frustrated.

## When to use

- "Send the user how to start a screen share so I can help them."
- "User agreed to a remote session — send them the join steps."
- "I need to see the user's screen to troubleshoot — send them the connect link/code steps."

## Steps

1. **Identify the client's remote-support tool first** (search_itglue / search_hudu / search_knowledge_base / search_tickets): the RMM's remote tool (ConnectWise ScreenConnect/Control, NinjaOne remote, Datto RMM, etc.), a standalone tool (TeamViewer, AnyDesk, Zoho Assist), or a built-in (Quick Assist on Windows, Teams screen share). The user experience differs — a code to read out, a link to click, an agent already installed that just needs approval, or a share button in a meeting. Note whether an unattended agent is already present (often the user just approves a prompt). If the tool is unknown, ask the technician ONE question.
2. **Confirm the session model from the ticket:** is the tech sending a link/code live, or does the user initiate? Draft the branch that matches so the steps line up with what the tech is actually doing.
3. **Draft the instruction block** to end-user rules, one action per step with what-you'll-see cues:
   - Calm framing first: "This lets me see your screen so I can fix it faster — you stay in control and can end it anytime."
   - The join path for the identified tool: click the link the tech sends / go to the documented short URL and type the code the tech reads you / find the already-installed helper icon and approve the prompt. Cue exactly what appears ("a small window will ask to allow the connection — click Allow").
   - The consent/control cues: what the "give control" or "allow" prompt looks like, that a session banner shows while connected, and how to end the session (the big Stop/End button) at any time.
   - Privacy note the user will appreciate: close anything private first, since the tech will see the whole screen.
   - Off-ramp: "If nothing happens after you click, or you don't see the prompt, stop and reply or stay on the phone with me — don't download anything a pop-up suggests."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Anti-scam framing is mandatory.** Remote-support tools are a classic scam vector; the draft tells the user to only ever start a session the desk itself requested, and never enter a code given by an unexpected caller. Never instruct downloading a remote tool from a link a pop-up or unknown caller provided.
- Tool match matters: ScreenConnect join steps sent to a TeamViewer shop don't apply.
- Do not include a live session code or connect URL fabricated by the guide — the tech supplies the real one; the guide describes where it goes.
- The "you can end it anytime / you stay in control" reassurance appears in every draft.
- No admin steps (deploying the agent, unattended-access config, RMM policy) in the user block. Note: the agent cannot run scripts or push software via RMM; this is purely the user-facing join.
- Localizable; version-cautious UI cues.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- end-user-guides/report-phishing-guide — because unsolicited "let me remote in" contacts are a scam pattern users should recognize.
- communication/email-baseline-standard.
