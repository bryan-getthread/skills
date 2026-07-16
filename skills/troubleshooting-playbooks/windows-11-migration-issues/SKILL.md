---
name: Windows 11 Migration Issues
description: Work post-upgrade tickets after a Windows 11 migration — driver regressions, default apps reset, printers vanished, features moved — with the rollback window checked first and treated as a deadline.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Windows 11 Migration Issues

The week after an upgrade wave, the desk gets the fallout: audio that died with an old driver, defaults quietly reset, printers that need rediscovering, and users lost in a moved Start menu. Two clocks matter — the rollback window (finite and shrinking) and the pattern clock: three tickets with the same regression means pause the wave, not fix the fourth machine.

## When to use

- "Ever since the Windows 11 upgrade, X is broken" — audio, display, Wi-Fi, dock, LOB app
- "My printer disappeared" / "my default PDF app changed" post-upgrade
- A machine that refuses to upgrade or was skipped by the rollout (safeguard hold)
- A user asking to go back to Windows 10, or a genuinely broken upgrade where rollback is on the table

## Steps

1. **History first.** search_tickets for this client filtered to since the upgrade wave began. The pattern is the diagnosis: five tickets naming the same model or same driver is a wave problem — flag it to whoever runs the rollout before treating ticket six as an individual fault.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the migration plan: target Windows 11 version, deployment method and ring schedule, hardware standards, known-issue log for the wave, LOB app compatibility notes.
3. **Establish the timeline and the rollback clock.** When did THIS machine upgrade, and is it still inside the rollback window (10 days by default before Windows deletes the previous installation — shorter if disk cleanup ran)? Check remaining eligibility early: if rollback is the likely outcome, it is a now-or-never decision, and the ticket's priority inherits that deadline.
4. **Identify versions.** Exact Windows 11 version/build, device model, and for the broken component: the driver version now vs what the vendor publishes for Windows 11 on this model (web_search the vendor's support page for the model — never assume the Windows-bundled driver is the right one).
5. **Evidence before theory.** The specific failure (Device Manager state, event-log entry, the app's error) and confirmation it actually correlates with the upgrade date — users attribute everything to the upgrade for a month; verify before accepting the framing.
6. **Branch:**
   1. **Driver regression** — audio/display/Wi-Fi/dock broken or degraded; Device Manager shows errors, generic drivers, or the vendor driver was replaced by an older/inbox one during upgrade. Guide: install the vendor's current Windows 11 driver for this exact model from the vendor's site (not third-party driver sites, ever). Escalate when: the vendor publishes no Windows 11 driver for this hardware — say plainly the component may be end-of-support on Win11; options are vendor confirmation, replacement hardware, or rollback while the window lasts. Multiple machines, same driver → pause-the-wave flag.
   2. **Default apps reset** — PDF, browser, mail handlers reverted. Quick per-user fix via Settings > Default apps; the real fix for a fleet is the deployment configuring defaults (managed policy) so ring 3 doesn't repeat ring 2's tickets. Escalate when: defaults are supposed to be policy-managed and didn't apply — deployment owner, not per-seat fixes.
   3. **Printer rediscovery** — printers missing or erroring post-upgrade. Re-add from the print server or direct IP per the client's documented standard; drivers may need the Windows 11 version. Escalate when: the driver itself has no Win11 build or the whole fleet's printers dropped — pair with the Printer Troubleshooting playbook and flag the wave.
   4. **LOB app breakage** — the vertical app misbehaves post-upgrade. Check the vendor's Windows 11 support statement for the app version in use (web_search) — running an unsupported combination is the first suspect. Escalate when: the app version predates Win11 support — that is an app upgrade conversation with the client and vendor (pair with the LOB Application Framework playbook), and rollback may be the interim.
   5. **UI disorientation** — "where did X go": Start menu, taskbar behaviors, right-click menus. Not a fault; a training gap. Answer the specific question, and if these tickets are frequent, recommend the rollout include a one-page "what moved" note for remaining rings.
   6. **Upgrade refused / machine skipped** — a safeguard hold (Microsoft blocking upgrade for a known incompatibility) or unmet hardware requirement. Identify which via the deployment tooling's readiness report or the vendor documentation for the block (web_search the specific hold). Escalate when: safeguard holds are never bypassed — the fix ships from Microsoft or the component vendor; hardware-ineligible machines go to the refresh list, not registry workarounds.
   7. **Rollback decision** — the machine is materially broken, no fix inside the window. Verify rollback eligibility, confirm with the user what returns (previous OS state) and what doesn't (changes made since upgrading may be lost — check what they've created since), and get the client contact's explicit go-ahead. Escalate when: the window has lapsed — rollback is now a reimage conversation with data-preservation planning, a different and bigger job.
7. **Close the loop.** Verify the specific broken thing works (sound plays, the printer prints, the LOB app opens its workflow), not just that the machine boots. Post a plain-text note: upgrade date, build, branch, driver/app versions before and after, fix or rollback decision and who approved, verification result — and flag wave-level patterns to the rollout owner explicitly.

## Guardrails

- Never bypass Microsoft safeguard holds or hardware-requirement checks via registry/media tricks on managed fleets — the hold exists because something breaks; the workaround inherits the breakage without the support.
- Rollback is user-data-sensitive and time-boxed: check the window early, confirm what the user changed since upgrading, and get explicit approval — a rollback that eats a week of new files is worse than the driver bug.
- Drivers come from the device vendor or the deployment tooling only — no driver-site roulette.
- Patterns beat heroics: repeated identical regressions get flagged to the rollout owner to pause the wave; do not silently fix the same thing fifteen times.
- No script or remote execution — remediation is guidance for the tech or user; use the RMM deep link for hands-on work when that integration is enabled. Do not invent driver versions, support statements, or safeguard-hold details — web_search and cite.
- search_itglue / search_hudu may be absent for this tenant — fall back to search_knowledge_base and say what you could not check.
- Notes destined for a PSA sync: plain text, no markdown or emojis.
