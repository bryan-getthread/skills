---
name: OneDrive Restore Guide
description: Draft reply-ready instructions for an end user to recover a deleted file or roll back to a previous version themselves in OneDrive/SharePoint — "send the user steps to restore their file."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# OneDrive Restore Guide

Produces a client-ready instruction block for self-service file recovery — recycle bin for deletions, version history for overwrites — scoped to where the file actually lived (personal OneDrive vs. a shared/SharePoint library), because the path differs and users rarely know which one they're in.

## When to use

- "User deleted a file — send them steps to get it back themselves."
- "User saved over a document and needs yesterday's version."
- Empowerment replies: teaching a repeat requester to self-serve recovery.

## Steps

1. **Verify the storage stack and the scenario first.** Confirm from the ticket and client docs (search_itglue / search_hudu / search_knowledge_base / search_tickets) that this client is on OneDrive/SharePoint (a client on a file server or another platform gets a different answer entirely — do not send this guide). Then pin down: deleted vs. overwritten, roughly when, and personal OneDrive vs. a shared "Documents/Teams" location. If the location is unclear, the draft's opening line asks the one distinguishing question ("Was this in your own OneDrive, or in a shared team folder?") and includes only the branch you can support.
2. **Deleted-file branch**, to end-user rules, one action per step with what-you'll-see cues:
   - Go in through the browser, not the desktop folder — give the plain route ("sign in to office.com the same way you do for webmail, open OneDrive").
   - Recycle bin location cue for the chosen branch (OneDrive left menu vs. SharePoint site's recycle bin), sort by delete date, tick the file, Restore — cue: "the file goes back to exactly where it was, not to your desktop."
   - Honest time bound: deleted items are recoverable for a limited window (about 93 days on Microsoft's current default — phrase as "roughly three months" and version-cautiously). If the deletion is older, the draft says to reply instead, because recovery moves to the admin side.
3. **Overwritten-file branch:** right-click the file (browser or File Explorer with OneDrive) → Version history → what-they'll-see cue ("a list of dates and times — each is a snapshot") → open the date they want → Restore. Include the reassurance that restoring an old version doesn't destroy the current one (it becomes another version).
4. Off-ramps in both branches: "If the file isn't in the recycle bin, or version history is empty or greyed out, stop and reply with the file name and roughly when it was lost — we have deeper recovery options on our side." Never mention or describe the admin-side second-stage recycle bin or backup restore in the user draft.
5. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- Stack verification is mandatory — recycle-bin steps sent to a file-server client are dead on arrival (that path is a backup-restore request; see troubleshooting-playbooks/backup-restore-request).
- Never promise recoverability ("it'll definitely be there") — the draft commits to the steps, not the outcome.
- If the ticket hints at mass deletion or ransomware-pattern loss (many files, strange extensions), do NOT send self-service steps — flag to the tech immediately; user-driven restores can destroy forensic state.
- Time-window numbers stay approximate and version-cautious; retention is tenant-configurable.
- No admin steps (site-collection recycle bin, restore-library, backup tools) in the user block.
- Localizable; degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled.

## Cross-references

- troubleshooting-playbooks/onedrive-sharepoint-sync — tech-facing when sync itself is the problem.
- troubleshooting-playbooks/backup-restore-request — when self-service can't reach it.
- communication/email-baseline-standard.
