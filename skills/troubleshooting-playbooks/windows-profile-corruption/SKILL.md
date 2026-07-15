---
name: Windows Profile Corruption
description: Handle "logged in to a temporary profile" and broken-profile tickets — confirm via the profile-service event IDs, choose repair vs rebuild deliberately, and run the data-preservation checklist before anything is deleted.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, get_ninjaone_device_link, add_ticket_note, web_search]
---

# Windows Profile Corruption

The temp-profile login is loud, but the wrong reflex — delete the profile and start over — destroys user data that was never backed up. This playbook confirms the state from the event log, tries repair before rebuild, and makes data preservation a mandatory gate, not an afterthought.

## When to use

- "<user> was logged in with a temporary profile" / "my desktop is empty and everything's gone"
- "User Profile Service failed the sign-in" errors
- Recurring temp-profile logins on one machine or one user
- Deciding whether to repair a profile or rebuild it

## Steps

1. **History first.** search_tickets for this user/device. A recurring temp-profile ticket means the prior "fix" didn't hold — find the cause (failing disk, security agent locking the hive, roaming-profile problems) instead of repeating the ritual.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the client's profile architecture: local-only, roaming, folder redirection, or profile-container tech (FSLogix/VDI) — a container-based profile follows that product's playbook, not the local-profile one, and misapplying the local fix there loses data.
3. **Confirm the state from the event log before theorizing.** Guide the tech to the Application log, User Profile Service events — 1511 (logged on with a temporary profile), 1515 (backup profile in use), 1521/1522 (cannot load/locate), 1530 (hive in use — often a scanner/agent holding it). The specific ID separates "profile is corrupt" from "profile was merely locked at logon", which needs no rebuild at all.
4. **Reassure and freeze.** Tell the user their data is almost certainly intact in the original profile folder — and that anything saved *during the temp session* lives in the temp profile and will vanish; have them save current work to a share/cloud location now.
5. **Branch:**
   1. **Locked, not corrupt** (1530-pattern, intermittent temp logins that self-resolve on reboot) — find the locker: AV/EDR scans at logon, backup agents, search indexing. Fix the interfering process schedule; do not rebuild. Escalate when: the locking agent is a managed security tool — coordinate with its owner rather than excluding profile paths unilaterally (that's a security decision).
   2. **Repair** (single event, hive intact) — guide the tech through the documented registry ProfileList repair: locate the user's SID entry, the `.bak` duplicate pattern, correct the ProfileImagePath/state per Microsoft's current guidance (verify steps with web_search — do this from documentation, not memory). Reboot, verify a normal login. Registry work is tech-only, never end-user guidance.
   3. **Rebuild** — criteria: repair failed, the hive itself is corrupt (1521/1522 persist), malware history in the profile, or the disk checks out but corruption recurs. Rebuild = new profile + data migration, executed only after step 6.
   4. **Underlying-cause check** — profile corruption is often a symptom: check disk health (SMART) before trusting any rebuild to a failing disk, and unexpected shutdown history (power loss corrupts hives). A rebuild on a dying disk is wasted work — pair with hardware-diagnostics and say so.
6. **Data-preservation checklist — mandatory before any rebuild/deletion.** Inventory and copy out, from the *old* profile path: Desktop, Documents, Downloads, Pictures; browser profiles (favorites, saved passwords — export properly, not by folder copy alone); mail data files (local PSTs; note OST re-syncs but signatures/autocomplete live in the profile); app-specific data per the client's LOB list from docs; anything the user names. Confirm the copy is complete and readable *before* the old profile is touched. Cloud-synced folders reduce risk but verify sync was healthy — a broken profile often had broken sync.
7. **Rebuild and restore.** New profile via a fresh first login (after removing the broken ProfileList entry per documentation), then migrate the preserved data in, reconnect mail/OneDrive/printers per the client's standard-setup doc. Never bulk-copy the old profile wholesale over the new one — that re-imports the corruption.
8. **Verify and note.** Two clean logins (including a reboot between) and user confirmation their data and mail are present. Plain-text note: event IDs observed, branch, preservation inventory, underlying cause if found, verification.

## Guardrails

- Nothing gets deleted before the data-preservation checklist is complete and verified — no exceptions, and say so to the tech in those words.
- Confirm via event IDs first; a temp-profile login without the event evidence gets diagnosis, not surgery.
- Registry repair steps come from current Microsoft documentation via web_search, are executed by the tech only, and are preceded by a registry/system restore point.
- Container/roaming/VDI profiles (per client docs) exit this playbook — flag and route to that product's procedure rather than improvising.
- No remote execution — hands-on work is the tech's, via get_ninjaone_device_link when NinjaOne is enabled.
- Notes for PSA sync: plain text, no markdown or emojis.
