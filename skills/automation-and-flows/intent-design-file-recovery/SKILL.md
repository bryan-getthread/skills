---
name: File Recovery Intent Design
description: Build the "I deleted/lost a file" intent — try self-service restore first (OneDrive/SharePoint recycle bin, version history), and capture where/when/which system to route a restore when self-service can't. Use when asked to "build a file recovery intent", "handle lost file requests", or when file-recovery shows up in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets, search_knowledge_base]
---

# File Recovery Intent Design

Guide the admin through building an intent for "I lost / deleted / can't find a file". Many of these the user can fix themselves — the OneDrive/SharePoint recycle bin or version history — so the intent tries self-service first, then captures the precise where/when/which-system needed to route a restore when self-service won't do it. Time matters (retention windows expire), so intake must be fast and complete.

## When to use

- "Build an intent for lost or deleted files."
- "File-recovery tickets never say where the file lived — fix the intake."
- Intent Mining flagged deleted/lost-file or "restore my document" requests.

## Design

**Trigger phrases (adapt to real ticket language):** "I deleted a file", "lost a document", "can't find my file", "accidentally deleted a folder", "restore a previous version", "my file is gone", "recover a spreadsheet", "overwrote my document", "file disappeared from SharePoint".
Near-miss watch: "my whole drive/computer won't open" is a device issue; "backup failed" is an infrastructure/backup issue — keep triggers scoped to *a specific file/folder* the user lost.

**Arguments to collect (fast, because retention expires):**
- Which file/folder (name, and file type if known).
- Where it lived: OneDrive, SharePoint/Teams site, a network/file share, or local device — this decides the restore path.
- When it was last seen / when it went missing (drives the recycle-bin vs. version-history vs. backup decision).
- What happened: deleted, overwritten, moved, or just can't-find.

**Reply flow (self-service first):**
1. If the file was in OneDrive/SharePoint and recently deleted → guide the user to the recycle bin (and second-stage recycle bin) or version history with the client's steps/KB link; ask "did that restore it?". A confirmed yes is a deflection.
2. If self-service restore is unavailable (network share, local device, past the recycle-bin window, or the user tried and it's not there) → create a ticket with all four arguments and route to the backup/restore workflow, flagged time-sensitive because retention windows expire.
3. Never promise a file is recoverable — restorability depends on retention, backup coverage, and how it was lost.

**Handoff rule:** backup/point-in-time restores, share-level recovery, and anything past the user-accessible recycle bin are technician/workflow actions. The intent deflects to self-service where it exists and otherwise captures a complete, time-flagged restore request.

**Variation hooks (per client):** which stores are self-service (OneDrive/SharePoint recycle bin steps), the backup product/retention for shares, the restore routing target, whether local-device files are backed up at all.

**Success metric:** deflection rate on the OneDrive/SharePoint self-service branch (files restored by the user without a ticket). Secondary: first-touch completeness on the escalation branch (location + timing + what-happened captured), since restore SLAs are retention-bound.

## Steps

1. `list_intents` — check for an existing file-recovery/backup intent; prefer `update_intent` and avoid overlap with the backup-failure intent.
2. `search_tickets` for past lost-file tickets; note how often self-service would have worked and what detail techs had to chase (location and timing → arguments).
3. `search_knowledge_base` for the client's OneDrive/SharePoint self-restore guide; reference it in the self-service reply.
4. Draft the full spec: triggers, arguments, self-service-first flow, routing target, variations, and a test plan (5 should-match, 3–5 should-not including a device-failure and a backup-failure near-miss). Show before any write.
5. On explicit confirmation: `create_intent`, then `set_variation_arguments` / `set_variation_replies` / `update_variation`.
6. Report what was created, restate the test plan, recommend activation only after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, degrade to a complete written spec for an admin to apply.
- Self-service first, but never promise recoverability — it depends on retention, backup coverage, and how the file was lost. Flag escalations time-sensitive.
- The intent does not perform restores beyond guiding the user's own recycle bin/version history; share and backup restores are technician/workflow actions.
- Do not invent the client's retention windows, backup product, or self-restore steps — placeholder anything unknown and flag it before activation.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text (PSA-safe, no markdown).
