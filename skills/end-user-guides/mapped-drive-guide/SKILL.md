---
name: Mapped Drive Guide
description: Draft reply-ready instructions for an end user to reconnect a missing network drive — "send the user steps to get their S: drive back."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Mapped Drive Guide

Produces a client-ready instruction block for the "my drive disappeared" ticket — the safe, user-doable reconnect steps, with the drive letter and path taken from the client's documentation, never guessed.

## When to use

- "Send <user> steps to reconnect their network drive."
- "S: drive shows a red X / is missing after reboot."
- Remote users whose drives only work on VPN and who don't know that.

## Steps

1. **Verify the drive facts first.** From client docs (search_itglue / search_hudu / search_knowledge_base) and prior tickets (search_tickets): which drive letter maps to which path for this user's role, whether drives are pushed automatically (GPO/Intune/script — the usual case) or manually mapped, and whether the share requires VPN when off-site. If the letter→path mapping isn't documented, ask the technician ONE question — a wrong path in a user's hands creates a second ticket.
2. **Order the draft by likelihood, cheapest fix first**, one action per step with what-you'll-see cues:
   - a. **The red-X wake-up:** open File Explorer, find the drive, double-click it — a drive with a red X often reconnects on first use. Cue: "the red X disappears and folders appear."
   - b. **Off-site? VPN first.** If the user is remote and the share needs VPN: "Connect to the VPN first (see the VPN steps we've sent before, or reply and we'll resend), then repeat step a."
   - c. **Restart** — sign out and back in (or restart), which re-runs the automatic mapping. Plain cue: "after signing back in, give it a minute, then check File Explorer."
   - d. Only if docs confirm drives are manually mapped for this client: the map-network-drive flow with the exact documented letter and path, including the "Reconnect at sign-in" checkbox, phrased plainly ("copy this location exactly, including the two backslashes").
   - Off-ramp after each stage: "If you see 'access denied' or a password box, stop and reply — that's a permissions issue on our side, and no amount of retrying will fix it."
3. Never include credential entry: if the share prompts for a username/password, that's the off-ramp, not a step — cached-credential fixes are tech-side.
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Never invent a UNC path or drive letter.** Docs or ask the tech — only those two sources. A user typing a guessed path gets an error you then have to untangle.
- If drives are auto-pushed for this client, do NOT include the manual-mapping step — a user manually mapping over a GPO drive creates conflicts that outlive the ticket.
- "Access denied" is a permissions problem — the draft must route it back to the desk, never suggest the user retry, use another account, or ask a colleague for credentials.
- No admin-side steps (no GPO, no share permissions, no server checks) in the user block — that's troubleshooting-playbooks/file-share-permissions territory.
- Localizable; the two-backslash cue matters for non-technical and non-English users — keep the copy-exactly framing.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled.

## Cross-references

- end-user-guides/vpn-connect-guide — the prerequisite for remote users.
- troubleshooting-playbooks/file-share-permissions — tech-facing when it's access, not connectivity.
- communication/email-baseline-standard.
