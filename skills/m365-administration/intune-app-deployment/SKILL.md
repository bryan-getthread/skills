---
name: Intune App Deployment
description: Process requests to deploy, update, or remove applications via Intune — packaging choice, required vs available intent, pilot-to-broad rings — with approval before any forced install or uninstall. Use for "push <app> to all machines", "make <app> available in Company Portal", or "remove <app> from the fleet".
category: M365 Administration
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, send_approval, schedule_ticket, web_search]
---

# Intune App Deployment

An app deployment request has four decisions hiding inside it: how the app is
packaged, who gets it, whether it is forced or offered, and how it will be
updated later. This skill makes all four explicit before anything is assigned.

## When to use

- "Deploy <application> to <client>'s machines."
- "Users should be able to install <app> themselves."
- "Update <app> everywhere — the old version has a vulnerability."
- "Uninstall <app> from the fleet."

## Steps

1. **Intake — pin the request down.** App name and exact version, licensing
   (is the client licensed for fleet-wide install?), source (vendor download,
   Microsoft Store, existing package), target: which client, which
   device/user groups, and deadline. search_itglue / search_hudu for the
   client's app standards and any existing package or install documentation.
2. **Choose the packaging path** (tech executes; agent records the choice and
   why): Microsoft Store app (winget-backed) when available — simplest and
   self-updating; MSI line-of-business for a plain MSI; Win32 (.intunewin) for
   anything with install logic, prerequisites, or an EXE installer. Note: avoid
   mixing LOB and Win32 installs during ESP on the same device. Define
   detection rules and install/uninstall commands as part of the package plan.
3. **Choose the intent honestly:**
   - **Required** — installs with no user choice. Use for security mandates
     and client-standard software. This is a user-visible forced change:
     approval gate applies.
   - **Available** — appears in Company Portal, user opts in. Default for
     convenience software; no forced footprint.
   - **Uninstall** — forced removal; treat with the same care as Required,
     plus a data check (does the app hold local user data?).
   Also decide user vs device assignment context and state it in the plan.
4. **Ring the rollout.** Pilot group (a handful of representative devices or
   IT staff) → validate install success, detection, and app function → broad
   group. For updates, supersedence (Win32) or a new version assignment
   follows the same rings. Schedule the broaden step (schedule_ticket) with a
   stated success criterion from the pilot (e.g., "≥95% install success, no
   new tickets referencing the app").
5. **Approval gate.** Before assigning Required or Uninstall beyond the pilot,
   send_approval to the client's documented authority with: app + version,
   intent, group and rough device count, install behavior the user will see
   (reboot? closing the app mid-use?), rollout schedule, and rollback
   (unassign; for updates, the prior version's package retained for
   re-deployment).
6. **Verify and document.** Success = install status report green across the
   ring and the app launches on a spot-checked device. Plain-text note: app,
   version, packaging type, intent, groups, pilot results, approver, rollback
   reference. For vulnerability-driven updates, record before/after version
   counts (label as point-in-time console figures).

## Guardrails

- No Required or Uninstall assignment to a broad group without recorded
  approval and a completed pilot — "it's just an app push" is how fleets get
  broken at 9am.
- Verify licensing before fleet deployment; deploying unlicensed software at
  scale is a compliance incident, not a favor.
- Vendor installers come from the vendor's official source only; record the
  download source and version in the note. Verify current packaging guidance
  against vendor docs (web_search) rather than memory — installers change.
- Keep the prior package until the new version's ring completes; rollback
  must be executable, not theoretical.
- Agent prepares the packaging/assignment plan and comms; the technician
  executes in Intune. Never report an assignment as live on intention.
