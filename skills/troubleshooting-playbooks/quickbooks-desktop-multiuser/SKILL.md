---
name: QuickBooks Desktop Multi-User
description: Work QuickBooks Desktop multi-user problems — H202/H505 hosting errors, -6000-series company-file errors, "someone else is in single-user mode", stuck locks — through the hosting-mode / Database Server Manager / file-integrity matrix.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# QuickBooks Desktop Multi-User

QuickBooks multi-user breaks in a small number of well-worn places: hosting mode turned on where it shouldn't be, the Database Server Manager not running on the host, firewall ports that changed with the QB year-version, and company-file damage. This playbook identifies which one before anyone touches the .ND or .TLG files.

## When to use

- "H202 / H505 error when switching to multi-user mode"
- "-6000, -XXX error opening the company file"
- "QuickBooks says someone else has the file open" / stuck single-user lock
- One workstation can open the file but others can't, or multi-user died after an update

## Steps

1. **History first.** search_tickets for this client + QuickBooks. Recurring H-series tickets usually mean a standing misconfiguration (workstation hosting, DHCP host IP), not a new fault.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the client's QB layout: which machine hosts the file, file path (UNC vs mapped drive), QB year-version, whether a full server or a peer workstation hosts.
3. **Identify versions before anything.** QB year and release (F2 window on any workstation), and whether QuickBooks Database Server Manager is installed on the host. Year matters: each QB year uses its own database service (QuickBooksDBxx) and its own firewall port range — web_search the exact ports for that year rather than reciting from memory.
4. **Evidence before theory.** Get the exact error code and which machines see it. H-series vs -6000-series is the primary fork: H-series is network/hosting, -6000-series is file access or file damage.
5. **Branch:**
   1. **Hosting-mode confusion** — multiple machines have "Host Multi-User Access" enabled. Exactly one machine (the file host) should host; every workstation should have hosting OFF. Guide the tech to check File > Utilities on each machine. Escalate when: hosting keeps re-enabling itself after being corrected — suspect an install-repair or GPO-imaged config and hand to the endpoint owner.
   2. **Database Server Manager / service** — H202/H505 with correct hosting. Verify QuickBooksDBxx and QBCFMonitorService are running on the host and the Database Server Manager has scanned the company-file folder. If the host's IP changed (DHCP), the .ND file points at a stale address — rescan; the durable fix is a reservation or static IP. Escalate when: services crash on start — capture the event-log entry and treat as an install-repair for the tech.
   3. **Firewall / name resolution** — service is up but workstations can't reach it. Guide: test the year-specific ports from a workstation, confirm the host resolves by name. Escalate when: the firewall is centrally managed — route the port exception to whoever owns that policy rather than local exceptions.
   4. **Company-file integrity (-6000 series)** — pair the specific -6000,-XXX code with Intuit's published meaning (web_search the exact code; do not guess). ND/TLG hygiene: renaming the .ND (and .TLG only alongside a verified backup) forces regeneration and clears many access errors — but the .TLG is the transaction log Intuit uses for data recovery, so never delete it, and never touch either while users are in the file. Verify/Rebuild Data is the vendor's own tool for logical damage. Escalate when: Rebuild reports errors it cannot fix or the file won't open at all — that is Intuit Data Services territory (vendor-only), and the client's accountant should know before any recovery attempt.
   5. **Stuck lock / phantom user** — "file is in use" with nobody in it. The QBW hosts the session list; guide: confirm all QBW32 processes are closed on every machine including the host, then reopen. Escalate when: locks recur without stale sessions — suspect antivirus scanning the company file live; route an exclusion request through the security owner, never disable AV.
6. **Close the loop.** Have a second workstation open the file in multi-user mode before resolving. Post a plain-text note: error code, branch, host machine and QB year, what changed, verification result.

## Guardrails

- No script or remote execution — every remediation is guidance for the tech or user; use the RMM deep link (get_ninjaone_device_link) for hands-on work when that integration is enabled.
- Never delete or rename the .TLG without a verified same-day backup; never touch .ND/.TLG while any user has the file open.
- File damage beyond Verify/Rebuild is Intuit's to fix — say so plainly and package the error codes for the vendor case rather than improvising repairs.
- This is accounting data: warn before anything that risks data state during payroll runs or period close, and prefer scheduling disruptive steps after hours.
- Do not invent error-code meanings, port numbers, or KB links — web_search and cite. search_itglue / search_hudu may be absent for this tenant; fall back to search_knowledge_base and say what you could not check.
- Notes destined for a PSA sync: plain text, no markdown or emojis.
