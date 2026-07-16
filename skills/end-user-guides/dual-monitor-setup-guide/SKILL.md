---
name: Dual Monitor Setup Guide
description: Draft reply-ready instructions for an end user to connect and arrange a second monitor — "send the user how to set up their second screen."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Dual Monitor Setup Guide

Produces a client-ready instruction block for a user connecting a second monitor and arranging it the way they expect — branched for Windows and Mac, and honest about the physical-cable checks that are the actual cause most of the time.

## When to use

- "User got a second monitor — send them how to set it up."
- "User's second screen isn't detected / is showing the wrong way — send arrangement steps."
- Return-to-office or new-desk tickets ("how do I use both screens?").

## Steps

1. **Note the platform and dock/laptop context** (search_tickets / search_knowledge_base for prior context). Confirm Windows vs. Mac and whether they're on a laptop with a docking station or a desktop, since docks add a layer. If unclear, draft both platform branches under headings and include the dock note.
2. **Draft the instruction block** to end-user rules, one action per step with what-you'll-see cues, physical checks first:
   - **Physical first (the real fix most of the time):** the monitor is powered on and set to the right input source; the cable is firmly seated at both ends; if on a dock, the dock is connected and powered. "Loose cable or wrong input is the most common reason a second screen stays black."
   - **Windows arrange:** right-click the desktop → Display settings → the two numbered boxes represent your screens → drag them to match how they physically sit → set which is the main display → choose "Extend these displays." Cue "Identify" to see which box is which.
   - **Mac arrange:** System Settings → Displays → arrange the screens by dragging → set the main display (the menu bar) → ensure it's set to extend, not mirror.
   - **The two things people actually want:** Extend (more space) vs. Duplicate (same on both) — explain in one plain line so they pick the right one. And dragging the boxes so the mouse crosses between screens in the direction the monitors actually sit.
   - The success test: drag a window from one screen to the other.
   - Off-ramp: "If the second screen still isn't detected after checking the cable and input, stop and reply — it may need a different cable/adapter or a dock we should look at."
3. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Cable/input/power checks lead every draft** — the majority of "second monitor not working" tickets are physical, and sending a user straight into display settings on a black screen frustrates them.
- Keep hardware cues generic — connector types (HDMI/DisplayPort/USB-C) and dock models vary; don't claim a specific port unless the ticket names the hardware.
- Explain Extend vs. Duplicate plainly — the wrong choice is the #2 cause after cabling.
- No admin/hardware-swap steps in the user block (driver updates, dock firmware, adapter procurement) — those are tech actions.
- Localizable; version-cautious Settings menu cues (Windows and macOS rename these panels).
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- end-user-guides/new-computer-first-day-guide — when the second monitor is part of a fresh desk setup.
- communication/email-baseline-standard.
