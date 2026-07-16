---
name: Group Policy Troubleshooting
description: Diagnose "GPO not applying" — drive mappings, lock screens, software installs, security settings missing — by reading gpresult output and walking the scope/filtering/inheritance ladder before touching any policy.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, liongard_launchpoint, liongard_metric, liongard_timeline, web_search]
---

# Group Policy Troubleshooting

The rule of this playbook: a GPO "not applying" is almost never a broken GPO — it is scope, filtering, inheritance, slow-link, or replication. Get a gpresult report and read which of those it is before proposing any edit.

## When to use

- "<user>'s drive mappings / printers / desktop settings didn't apply"
- A new GPO was created but machines aren't picking it up
- Settings apply to some users/machines but not others
- "Group Policy processing failed" events (1058, 1030, 7016, 8194) in the ticket

## Steps

1. **History first.** search_tickets for this client on GPO/drive-mapping/policy symptoms. One machine → local processing problem. Many machines since a date → a policy or infrastructure change on that date; ask what changed (new GPO, DC work, VPN change) before anything else.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the client's AD layout: OU structure, naming convention for GPOs, which DCs exist, and whether users work over VPN (slow-link matters). If Liongard's Active Directory inspector is enabled, use liongard_launchpoint to confirm it last ran clean, then liongard_metric / liongard_timeline for GPO inventory and recent GPO changes — state the dataprint age. If absent, rely on docs and the tech's console access.
3. **Get the report before theorizing.** Guide the tech to run `gpresult /h report.html` (as the affected user, on the affected machine; `/scope computer` needs elevation) or `gpupdate /force` then gpresult. The report is the truth: Applied GPOs, Denied GPOs with the reason, and the processing time/link speed.
4. **Read the Denied reason — it names the branch.**
   1. **Not in scope (GPO absent from both lists)** — the GPO isn't linked to an OU in the user's/computer's path. Verify which OU the object actually sits in (users moved between OUs is the classic). Fix is linking or moving the object per the client's OU convention — confirm with the client's AD owner before moving anything. Escalate when: the OU design itself is the problem (flat OU, everything in default Computers container — computers there get no OU-linked GPOs).
   2. **Denied (Security Filtering)** — object isn't in the filtered group, or (post-MS16-072) "Authenticated Users" / "Domain Computers" lost Read on the GPO. Check group membership AND delegation-tab Read. Escalate when: the filtering group is managed by another team or an IAM tool.
   3. **Denied (WMI Filter)** — the WMI filter evaluates false on this machine (OS version filters go stale after upgrades). Test the WMI query on the machine before concluding.
   4. **Denied (Blocked / overridden)** — Block Inheritance on the OU, or a higher Enforced GPO wins, or "loopback processing" changes which user settings apply on that machine (RDS/AVD hosts almost always use loopback — check before declaring a user GPO broken there). Map the precedence in the report's order; do not guess.
   5. **Slow link detected** — report shows a slow link or the settings that failed are the bandwidth-gated types (software install, folder redirection, scripts, drive maps under some configs), typical on VPN. These only apply on fast links or at logon over fast links. Set expectations honestly: the fix is policy design (item-level targeting, Always On VPN timing), not repeated gpupdate.
   6. **Processing errors (events 1058/1030: cannot read gpt.ini/SYSVOL)** — this is not a GPO problem, it is SYSVOL access or replication. Check whether the machine can reach `\\<domain>\SYSVOL`, and compare the GPO's version on two DCs (or check DFSR health). If versions differ across DCs → hand off to the **ad-replication-issues** playbook; do not edit the GPO to "fix" it.
5. **Verify and note.** After the fix, `gpupdate /force`, sign out/in (user settings) or reboot (computer settings — some, like software install, only apply at boot), then a fresh gpresult showing the GPO in Applied. Plain-text note: symptom, gpresult finding (denied reason or event IDs), branch, action taken or handed off, verification result.

## Guardrails

- Never edit, unlink, or re-permission a GPO as an exploratory step — every change hits every object in scope. Diagnose read-only; make one targeted change with the client's AD owner aware.
- Never enable Block Inheritance or Enforced to "make it work" — that trades one mystery for a permanent one.
- gpresult/gpupdate are guidance for the tech or an attended user — no remote script execution from here.
- If the same GPO shows different versions on different DCs, stop: that is replication, not policy. Route to ad-replication-issues.
- Do not recite policy CSE behavior from memory when it decides the diagnosis — verify against Microsoft's current docs (web_search), especially for drive-map/loopback/slow-link rules.
- Docs and Liongard coverage vary per tenant — note anything you could not check.
