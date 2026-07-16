---
name: Teams Meeting Guide
description: Draft reply-ready instructions for an end user to join and run a Teams meeting — audio/camera checks, screen sharing, recording basics — "send the user steps for their Teams meeting."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Teams Meeting Guide

Produces a client-ready instruction block for the anxious-meeting-host ticket: how to join cleanly, check audio before anyone hears them, share a screen, and (policy permitting) record — sized to what the user actually asked, not a full Teams manual.

## When to use

- "User has a big Teams meeting tomorrow — send them a how-to."
- "Send <user> steps to share their screen / record the meeting."
- Post-incident follow-up after a meeting went badly ("couldn't be heard," "shared the wrong screen").

## Steps

1. **Verify the environment first** (search_tickets / search_itglue / search_hudu / search_knowledge_base): the client actually runs Teams (not another meeting platform), whether the user has the desktop app or joins via browser, and — if recording is in scope — whether the client's policy permits recording for regular users. If the recording policy is unknown and the user asked about recording, ask the technician ONE question rather than promising a button that may be disabled.
2. **Draft only the sections the user needs** (join / audio / sharing / recording), each to end-user rules — one action per step, what-you'll-see cues:
   - **Join:** click the meeting link in the calendar invite; cue the pre-join screen ("you'll see yourself on camera with buttons for mic and camera BEFORE anyone can see or hear you — nothing is live yet"). This one cue removes most meeting anxiety.
   - **Audio check:** on the pre-join screen, confirm the named mic/speaker matches their headset; the "test call"-style check where available, phrased version-cautiously ("look for a settings gear on the pre-join screen"). Off-ramp: "If your headset isn't in the list, stop and reply before the meeting — that's usually a quick fix on our side."
   - **Sharing:** the share button cue, and the single most important distinction in plain words: sharing one **window** (they only ever see that window — safest) vs. the whole **screen** (they see everything, including notifications). Recommend window-share by default; suggest closing email and chat first regardless.
   - **Recording (only if policy-verified):** where the record option lives (the "…" more menu), the cue that everyone is notified recording started, where the recording lands afterwards in plain terms, and the etiquette line ("announce you're recording"). If policy blocks it, the draft says so plainly instead of describing a button they don't have.
3. Add the day-of safety net: "Join 5 minutes early. If anything looks different from these steps, reply here or call the desk — before the meeting, not during."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- Never describe features the client's policy disables (recording, external sharing) — verify or ask. A missing button the guide promised is a credibility hit.
- Keep it to what was asked — a 30-step everything-guide is where users stop reading. Sections not requested get one offer line ("Want a screen-sharing walkthrough too? Reply and we'll send it").
- Version caution: Teams UI shifts constantly — cues by name and purpose ("the camera button"), never by position or exact icon shape.
- No admin steps (meeting policies, org settings) in the user block.
- Recording carries consent/legal weight in some jurisdictions — the announce-it line stays in every recording section; never advise covert recording.
- Localizable; degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled.

## Cross-references

- troubleshooting-playbooks/teams-issues — tech-facing diagnostics when Teams itself misbehaves.
- communication/email-baseline-standard.
