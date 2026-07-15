---
name: OneDrive / SharePoint Sync
description: Diagnose OneDrive and SharePoint sync problems — stuck "processing changes", missing files, sync errors, red X icons — separating client-state, library-limit, and permission causes before ever suggesting a reset.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# OneDrive / SharePoint Sync

Most sync tickets are misdiagnosed permissions or library-limit problems, and most resets are unnecessary. This playbook reads the client's actual state, checks the library against known limits, isolates permissions, and reserves reset/unlink for defined criteria.

## When to use

- "OneDrive is stuck on processing changes" / red X or paused icon
- "Files I saved aren't showing up for <user> / the team"
- Sync errors naming specific files or paths
- "A whole library stopped syncing after a reorganization"

## Steps

1. **History first.** search_tickets for this user and this library. Same library across users → server-side (permissions, library change, retention/locks). One user only → client-side.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the client's file architecture: which libraries are meant to sync vs use shortcuts/on-demand, known-large libraries, folder redirection / Known Folder Move policy.
3. **Identify versions.** OS and OneDrive client version (and whether it's the per-machine or per-user install). Old clients cause known, already-fixed failures — check before deeper work; never assume it's current.
4. **Read the client state before theorizing.** Guide the tech/user: the OneDrive icon state, the error list in the activity center (exact file names and error text), and whether the account shown is the right one. "It's not syncing" must become a specific error or a specific stuck state.
5. **Branch:**
   1. **Sync client state** — paused, signed out, throttled ("we're processing a large number of changes"), or crashed. Guide: resume/re-sign-in; for a genuinely stuck-but-healthy client, closing and restarting OneDrive is the first move, not reset. If stuck on one file, that file (open handle, path, size) is the suspect — see branch 2/4.
   2. **Library limits and item hygiene** — errors on specific items. Check the documented limits: path length, invalid characters/names, file size cap, and total item count in the synced scope (very large libraries degrade and should move to shortcut/on-demand patterns rather than full sync). The fix is restructuring or scoping what syncs, and be honest that this is a design fix, not a toggle. Escalate when: the client's whole file architecture exceeds sane sync scope — propose the architecture conversation, don't band-aid per user.
   3. **Permissions vs sync conflicts** — "files missing" is usually one of these two, and they look identical to the user. Permissions: can the user see the file in the browser? If no → access problem (pair with the file-share/permissions playbook — same logic applies to SharePoint groups/links). If yes but not locally → true sync issue. Conflicts: look for conflict-copy files and check whether two people edit the same file offline; the remedy is workflow (co-authoring in supported formats), and say so.
   4. **Reset criteria** — reset (or unlink/relink) ONLY when: client state is corrupt (repeated crash, phantom errors on files that don't exist), stuck > 24h after restart with no named-file errors, or Microsoft's own guidance for the observed error says reset. Before any reset: confirm the error list shows no unsynced local-only changes, and have the user copy any recently changed files aside — reset re-downloads state and unsynced work is at risk. Say this plainly.
6. **Verify and note.** Create a test file both directions (local → cloud, cloud → local) and confirm arrival. Plain-text note: state observed, branch, action, reset yes/no and why, verification.

## Guardrails

- Reset/unlink is last resort and gated by the criteria above — never the first suggestion, and never without the data-preservation step.
- Never tell a user to delete local folders to "clean up" sync — that deletion can propagate. Any destructive-looking step gets an explicit are-changes-safely-uploaded check first.
- No remote execution — all steps are guidance for the tech or user.
- If the failure is a Microsoft service incident, say only Microsoft can act and reference the incident rather than churning client-side.
- Docs tools vary per tenant — note what you could not check.
- Notes for PSA sync: plain text, no markdown or emojis.
