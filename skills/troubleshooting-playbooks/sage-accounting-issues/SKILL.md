---
name: Sage Accounting Issues
description: Work Sage 50 / Sage 100 problems — can't open company data, Pervasive/Actian errors, one workstation broken after an update, painfully slow postings — through the data-path / database-engine / version-parity matrix, with period-close pressure in mind.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Sage Accounting Issues

Sage 50 and Sage 100 tickets cluster around four causes: the path to the shared data, permissions on it, the database engine underneath (Pervasive/Actian Zen for Sage 50, and per-edition engines for Sage 100), and version mismatch between workstations and the data host. This playbook isolates which layer — and respects that these tickets spike at period close, when the tolerance for downtime is zero.

## When to use

- "Sage can't open the company" / "file system error" / a Pervasive or Actian status code
- One workstation lost Sage after a Windows or Sage update; the rest are fine
- Sage is suddenly very slow to post or run reports
- "We're closing the month/quarter and Sage is down" — priority-sensitive

## Steps

1. **History first.** search_tickets for this client + Sage. A matching recent ticket often holds the exact fix; repeated engine-error tickets suggest a host problem, not per-user issues.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the Sage layout: product and edition (Sage 50 vs Sage 100, and 100's edition matters), data path (server, share, mapped drive vs UNC), which machine runs the database engine, version/build standard.
3. **Read the calendar.** Ask whether the client is in period close, payroll, or year-end. If yes, treat as high urgency, avoid disruptive steps during business hours, and say explicitly which steps can wait.
4. **Identify versions.** Sage product, version and build on the affected workstation AND on the data host or a working peer. A workstation updated past (or lagging behind) the data version is one of the most common single-machine failures — the fix is parity, per the vendor's supported path.
5. **Evidence before theory.** Exact error text and any engine status code. Pervasive/Actian codes are specific — web_search the exact code with the product name; a network-class code and a file-permission-class code lead to different branches. Do not proceed on symptom description alone.
6. **Branch:**
   1. **Data path** — "cannot find/open company". Verify the path Sage is pointed at still exists and matches docs; mapped-drive letters that vanish at logon (GPO change, VPN not up) are the classic cause. Prefer the documented path style — switching a client between mapped and UNC ad hoc creates the next ticket. Escalate when: the data was moved or the server renamed — that is a coordinated reconfiguration of every workstation, not a quick fix.
   2. **Permissions** — opens for some users, "access denied" or engine file errors for others. Users need modify rights on the entire data folder (Sage writes lock and temp files beside the data). Ladder share vs NTFS like any share issue. Escalate when: permissions were recently "hardened" by a security change — coordinate with whoever made it rather than quietly widening access.
   3. **Pervasive/Actian layer (Sage 50)** — engine status codes, everyone down. Verify the engine service is running on the host and workstations can reach it; a network-class status code with the service up points at firewall or name resolution. Escalate when: the engine crashes repeatedly or data files are flagged damaged — vendor-guided repair only; Sage support owns data repair, and the client's accountant should know before recovery is attempted.
   4. **Performance** — postings and reports crawl. Ask what changed: antivirus now scanning the data share (route an exclusion request through the security owner — never disable AV), wireless vs wired workstation, data file size at year boundaries. Escalate when: slowness is host-wide and correlates with disk or memory pressure — that is a server-capacity conversation, not a Sage setting.
7. **Close the loop.** Have the user open the company, post a test transaction or run the failing report before resolving. Post a plain-text note: product/version, error/status code, branch, fix or handoff, verification result — and note if work was deferred for period close.

## Guardrails

- No script or remote execution — remediation is guidance for the tech or user; hand off hands-on work via the RMM deep link when that integration is enabled.
- Never run Sage data repair/verification utilities against live data without a verified backup and an after-hours window; vendor-guided only for anything touching data files.
- Period-close awareness is part of the job: ask, flag urgency honestly, and defer disruptive steps rather than gambling with an accounting deadline.
- Do not guess status-code meanings or supported upgrade paths — web_search against Sage/Actian documentation and cite. If only the vendor can fix it, say so and build the escalation package (see the LOB Application Framework playbook).
- search_itglue / search_hudu may be absent for this tenant — fall back to search_knowledge_base and say what you could not check.
- Notes destined for a PSA sync: plain text, no markdown or emojis.
