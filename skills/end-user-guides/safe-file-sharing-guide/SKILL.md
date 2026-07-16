---
name: Safe File Sharing Guide
description: Draft reply-ready instructions for an end user to share files the approved way — links over attachments, the right audience setting, external-sharing rules — "tell the user how to share this file properly."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Safe File Sharing Guide

Produces a client-ready instruction block for sharing files the way the client's policy intends — the approved sharing method, the audience choice that trips everyone up ("anyone with the link" vs. "specific people"), and the external-recipient path, verified against what the client's tenant actually allows.

## When to use

- "User asked how to send a big file to a client — send them the right way."
- "User keeps emailing spreadsheets as attachments — send the share-a-link guide."
- After an oversharing incident: the education reply on doing it correctly next time.

## Steps

1. **Verify the client's sharing platform and policy first** (search_itglue / search_hudu / search_knowledge_base / search_tickets): OneDrive/SharePoint links (the common case) or another sanctioned tool; whether sharing to people outside the company is enabled, blocked, or restricted; the client's default link audience; and any documented banned channels (personal Dropbox/Gmail, consumer transfer sites). If the policy is unknown — especially the external-sharing question — ask the technician ONE question; telling a user to "just share externally" against a blocking policy produces a confusing error, and describing a looser policy than reality is worse.
2. **Determine the scenario from the ticket:** internal colleague, external recipient, or a too-big-for-email attachment. Draft only the branch that applies.
3. **Draft the instruction block** to end-user rules, one action per step with what-you'll-see cues:
   - The core habit, framed as a benefit not a rule: "Send a link, not a copy — everyone sees the current version, you can un-share later, and no mailbox fills up."
   - The share flow on the client's platform: the Share button on the file (cue what the dialog looks like), then **the audience decision made explicit** — "the box near the top says who the link works for; click it and choose. 'People you choose' is the safe default; 'Anyone with the link' means exactly that — anyone it gets forwarded to."
   - View vs. edit in one plain line ("can they change the file, or just read it? Pick before you send").
   - **External branch (only if policy-verified as allowed):** type the outside person's email address in the share dialog rather than making an open link; the cue for what the recipient experiences ("they'll get an email from the system and may be asked to enter a code — that's normal, it proves they're them"). If external sharing is blocked, the draft says so plainly and gives the client's documented alternative — never a workaround.
   - The don'ts, brief: no personal cloud accounts or file-transfer sites for work files, and don't make an "anyone" link just because it's fewer clicks.
   - Off-ramps: "If the sharing box won't accept the outside address, stop and reply — that's a policy setting, not a mistake you made." / "If you're sharing anything sensitive (payroll, health, contracts) and aren't sure, reply first — thirty seconds of asking beats un-sharing later."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **Policy verification before drafting is the skill.** The external-sharing capability differs per client tenant; a guide that contradicts the tenant's settings teaches users the desk doesn't know its own systems.
- Never instruct a workaround for a blocked path (personal accounts, consumer transfer sites, zipping-to-evade) — if the policy blocks it and the business need is real, that's a ticket for the tech/admin, and the draft says so.
- The audience-setting explanation appears in every draft — "anyone with the link" chosen by accident is the most common data-exposure root cause a desk sees.
- Sensitive-data hesitation off-ramp stays in; this guide never green-lights sharing regulated data — that's a policy/compliance call above the guide's pay grade.
- No admin steps (tenant sharing settings, DLP, sensitivity labels admin) in the user block.
- Localizable; version-cautious dialog cues.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled.

## Cross-references

- end-user-guides/onedrive-restore-guide — the recovery counterpart on the same platform.
- troubleshooting-playbooks/file-share-permissions — tech-facing when sharing fails for permission reasons.
- communication/email-baseline-standard.
