---
name: OneDrive Known Folder Move
description: Work the ticket wave from a Known Folder Move rollout — "where did my desktop go", sync conflicts, path-length and invalid-character legacy files, duplicate Desktop confusion — without ever "fixing" a machine by unhooking KFM.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# OneDrive Known Folder Move

KFM redirects Desktop, Documents, and Pictures into OneDrive — and the tickets it generates are mostly perception ("my files are gone" when they moved) plus a predictable tail of legacy-file friction: paths too long, characters OneDrive won't take, and conflicts on machines where the same user had different local copies. The one forbidden fix: unhooking KFM per-machine to make a ticket go away — that forks the user's files into two realities. For general sync-client failures unrelated to KFM, use the OneDrive & SharePoint Sync playbook.

## When to use

- "My desktop files disappeared" or "my documents are gone" right after a KFM rollout hits a machine
- OneDrive stuck on specific files with path/character/size errors since folders moved
- Duplicate folders ("Desktop" and "Desktop on <device>", conflict copies) after KFM applied on multiple machines
- A user or tech asking to turn KFM off on one machine because it's "causing problems"

## Steps

1. **History first.** search_tickets for this client + OneDrive/desktop/documents since the rollout date. Volume and clustering tell you whether this is one machine's friction or a comms failure across the wave — many "files gone" tickets in week one means the rollout needed (and still needs) an announcement, which is cheaper than the tickets.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the KFM rollout plan: which folders are redirected, policy-enforced (silent) vs user-prompted, rollout schedule, and any documented exclusions.
3. **Identify versions and state.** OneDrive client version on the machine (KFM behavior and file-name support improved across versions — an outdated client causes solved problems), Windows version, and the machine's actual KFM state: are the known folders redirected on THIS machine yet, or is it mid-wave? Half the confusion is machines in different states.
4. **Evidence before theory.** OneDrive's own error list (the client names each failing file and why), the sync status icon state, and for "missing files": where they actually are — check the user's OneDrive on the web first. Files that moved are in OneDrive; files genuinely absent are a different, more serious ticket.
5. **Branch:**
   1. **"Where did my files go"** — the folder moved, the user's mental map didn't. Show them: Desktop/Documents now live under OneDrive and sync everywhere; nothing is lost, and the web view proves it. If these tickets are arriving in bulk, escalate the comms gap to the rollout owner — a one-paragraph user note ("your Desktop now follows you") prevents the rest of the wave. Escalate when: the file is NOT in OneDrive web, local, or the OneDrive recycle bin — stop reassuring and treat as potential data loss: check the site recycle-bin tiers and restore options before saying anything is unrecoverable.
   2. **Path length / invalid names** — the client flags files it cannot take up: full paths beyond the documented limit, forbidden characters, files like desktop.ini oddities, or items over size limits. These are legacy-file hygiene, fixed by renaming/shortening/restructuring — guide the user through the client's error list item by item (the list is finite; zero it). Escalate when: hundreds of failing items or deep nested structures from an old file-server migration — that is a cleanup task to schedule with the user/client, not a live-call fix.
   3. **Sync conflicts / duplicates** — "Desktop on <old device>" folders, conflict-copy files, or two machines that had diverged local folders before KFM unified them. Explain the mechanism (both copies were preserved — nothing was chosen for the user), then help merge: newest-wins only by the user's judgment, never the desk's; the user decides which version of each conflicted file survives. Escalate when: business-critical files with material divergence (two edited versions of the books) — the user's manager or the file's owner arbitrates, and both versions stay until they do.
   4. **KFM won't apply / errors applying** — policy pushed but folders won't redirect: usual causes are an old client version, a known folder containing something unmovable (see branch 2 blockers), or the folder redirected elsewhere by legacy folder-redirection GPO. The GPO collision is the classic: on-prem folder redirection and KFM fight — resolution belongs to the deployment owner, per Microsoft's documented migration path (web_search current guidance; cite). Escalate when: GPO collision or tenant-policy misconfiguration — deployment owner with the specific error; per-machine hacks make the fleet inconsistent.
   5. **"Just turn it off for this machine"** — refuse the reflex. Unhooking KFM on one machine while the fleet policy expects it forks the user's files (local-only again) and recreates the unprotected-Desktop problem KFM exists to solve. If the machine has a genuine incompatibility, the exception goes through the rollout owner as a documented policy exclusion. Escalate when: always — exceptions are the rollout owner's call, not a desk-side setting change.
6. **Close the loop.** OneDrive icon green (no failing items), the user locates their files in the new location themselves (that confirms the mental map is fixed, not just the sync), and any conflict list is zeroed or explicitly parked with the user. Post a plain-text note: machine's KFM state, branch, evidence (error list items, where files were found), what remains for the rollout owner, verification result.

## Guardrails

- Never disable or unhook KFM per-machine to close a ticket — exceptions are policy decisions for the rollout owner, documented and deliberate.
- Never delete "duplicate" folders or conflict copies on the user's behalf, and never pick which version of a conflicted file wins — preserve both until the user (or the file's owner) chooses.
- Treat "file not found anywhere" as potential data loss: exhaust OneDrive web, recycle-bin tiers, and restore options before declaring anything gone, and escalate rather than guess.
- Comms is remediation: recurring "files gone" tickets are a rollout-communication defect — flag it; do not just absorb the wave.
- No script or remote execution — remediation is guidance for the user or tech; deployment/GPO changes belong to the rollout owner. Do not invent limits or policy behaviors — web_search current Microsoft documentation and cite.
- search_itglue / search_hudu may be absent for this tenant — fall back to search_knowledge_base and say what you could not check.
- Notes destined for a PSA sync: plain text, no markdown or emojis.
