---
name: VPN Connect Guide
description: Draft reply-ready instructions for an end user to connect to their company VPN using the client's actual VPN software — "send the user steps to get on the VPN."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# VPN Connect Guide

Produces a client-ready instruction block for connecting to the VPN — written for the specific VPN client this customer runs (GlobalProtect, FortiClient, Cisco Secure Client/AnyConnect, SonicWall NetExtender, WatchGuard, OpenVPN, Meraki, or another). There is no such thing as a generic VPN guide; the software name and connect flow differ everywhere.

## When to use

- "Send <user> instructions to connect to the VPN."
- Remote-work or first-day-remote tickets ("working from home tomorrow, how do I get on the VPN?").
- "User says the VPN 'isn't working' — send them the correct connect steps first."

## Steps

1. **Identify the client's VPN software and portal first.** Search client docs (search_itglue / search_hudu / search_knowledge_base) and prior VPN tickets (search_tickets) for: the VPN client name, whether it's pre-installed on managed machines (usual) or user-installed, the portal/server name the user selects (from docs only — NEVER invent or guess a hostname), and the sign-in style (same work credentials + MFA, or certificate/always-on where the user does nothing). If any of these are unknown, ask the technician ONE question before drafting.
2. If docs show the VPN is **always-on or auto-connecting**, the correct draft is short: how to check it's connected (icon state cue) and the off-ramp — do not send a manual connect procedure for a VPN the user should never touch.
3. **Draft the connect flow** for the identified product, to end-user rules:
   - Find the app: name it exactly and describe the icon and where it lives (system tray / menu bar), one action per step.
   - What-you'll-see cues: what the disconnected icon looks like, what the sign-in window asks for, what MFA prompt to expect, and what "connected" looks like (icon change, status word).
   - The success test: one plain check that proves they're on ("open <the documented internal resource cue> — if it loads, you're connected"). Use only a check the client's docs support; otherwise use the client's icon state alone.
   - Off-ramps: "If the app isn't in the list at all, stop and reply — we'll install it for you." / "If sign-in fails twice, stop — don't keep retrying — and reply with a photo of the message."
   - Home-network reality note: "Hotel and café wifi sometimes blocks VPNs — if it works at home but not on public wifi, that's the network, not your account."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Never invent a portal address, server name, or profile name.** These are environment-specific; a guessed hostname is worse than no guide. Docs or ask — those are the only two sources.
- Product match mandatory; a GlobalProtect walkthrough on a FortiClient shop fails at step one.
- No admin-side or firewall-side steps in the user block; no instructions to reinstall, change adapter settings, or edit profiles — those are tech actions (see troubleshooting-playbooks/vpn-troubleshooting).
- Never include shared secrets, pre-shared keys, or certificate files.
- Repeated failed VPN sign-ins can lock the account — the stop-after-two off-ramp stays in every draft.
- Localizable; version-cautious UI cues.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- troubleshooting-playbooks/vpn-troubleshooting — the tech-facing diagnostic when the user followed these steps and it still fails.
- end-user-guides/mapped-drive-guide — the most common "why I need the VPN" follow-on.
- communication/email-baseline-standard.
