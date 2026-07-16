---
name: Bluetooth Device Pairing Guide
description: Draft reply-ready instructions for an end user to pair a Bluetooth headset, mouse, or keyboard with their computer — "send the user how to pair their headset/mouse."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Bluetooth Device Pairing Guide

Produces a client-ready instruction block for a user pairing a Bluetooth accessory — headset, mouse, or keyboard — with their work computer, branched for Windows and Mac and honest about the one step everyone misses: putting the accessory into pairing mode.

## When to use

- "Send the user how to pair their new headset."
- "User's Bluetooth mouse/keyboard won't connect — send pairing steps."
- New-accessory or hybrid-work tickets ("got a headset for calls, how do I connect it?").

## Steps

1. **Note the platform and, if known, the accessory** (search_tickets / search_knowledge_base for prior context; check whether the client has any policy on Bluetooth accessories — some managed/secure environments restrict Bluetooth, in which case the answer is "reply to the desk," not a pairing walkthrough). Confirm Windows vs. Mac from the ticket. If the machine's Bluetooth is disabled by policy, don't send self-service steps — flag to the tech. If platform is unclear, draft both branches under headings.
2. **Draft the instruction block** to end-user rules, one action per step with what-you'll-see cues:
   - **The universal first step, called out clearly:** put the accessory into pairing mode — this is what people miss. Describe the common pattern generically ("hold the power/Bluetooth button until the light flashes fast — flashing means it's discoverable; check the little card that came with it for the exact button"). Charge/power it first.
   - **Windows:** Settings → Bluetooth & devices → make sure Bluetooth is On → Add device → Bluetooth → pick the accessory from the list when it appears → confirm any code. Cue the list and the "connected" state.
   - **Mac:** System Settings → Bluetooth → turn on if needed → the accessory appears under "Nearby Devices" → Connect. Cue the same.
   - **The headset-audio gotcha:** after pairing, in a call app the user may still need to pick the headset as the microphone/speaker — name where (Teams/Zoom audio settings, or the system sound output). This is the second-most-common "it's paired but I can't hear" cause.
   - The success test: play a sound / move the mouse / type to confirm it works.
   - Off-ramp: "If the accessory never shows up in the list, stop and reply — it may need charging, may still not be in pairing mode, or Bluetooth may be turned off for your work laptop, which we can check."
3. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **The pairing-mode step is the point** — a walkthrough that skips "make it discoverable first" fails for most users; it appears prominently in every draft.
- Keep device cues generic — button placement and light patterns vary by brand; avoid pixel-precise or brand-specific claims unless the ticket names the exact model.
- Check for a Bluetooth-restriction policy before sending self-service steps; in restricted environments this is a tech action, not a user one.
- The "select it as your mic/speaker in the call app" note stays in for headset cases.
- No admin steps (driver installs, Bluetooth stack troubleshooting, registry/policy) in the user block.
- Localizable; version-cautious Settings menu cues (Windows and macOS rename these panels).
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- end-user-guides/teams-meeting-guide — when the driver is getting a headset working for meetings specifically.
- communication/email-baseline-standard.
