---
name: Sage 50 / Sage 100
description: Diagnose Sage 50 and Sage 100 problems — data-path and share-permission faults, the Pervasive/Actian (PSQL) database engine service, and multi-user access errors — under period-close time pressure, from the connection manager and engine state, never by touching company data blindly. For general Sage guidance see sage-accounting-issues.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Sage 50 / Sage 100

Sage 50 and Sage 100 both sit on a shared data path served by the Actian/Pervasive PSQL engine, and most "Sage is broken" tickets are really the data path, the engine service, or share permissions — not the accounting app. This playbook works that matrix, mindful that these tickets often land during period close when downtime is expensive. For product-agnostic Sage guidance, sage-accounting-issues is the parent.

## When to use

- "Can't connect to the company / data" , "unable to open company", or workstations can't reach the shared data
- Pervasive/Actian PSQL errors, the engine service not running, or "Btrieve" / status-code errors
- One workstation opens the company but others can't; multi-user access broke after a change
- Slowness or lock errors during month-end/year-end close

## Steps

1. **Product, version, and data path first.** search_itglue / search_hudu / search_knowledge_base for the layout: Sage 50 vs Sage 100 and the year/version, which machine **hosts** the data and the exact data path (UNC vs mapped drive — Sage is picky here), the Actian/Pervasive **PSQL version** and whether the server runs the engine in server (not workstation) mode, how workstations point at the data (the connection manager / data path config), and the share + NTFS permissions on the data folder. Establish the host and data path before anything. If a Liongard Windows inspector runs, corroborate service/share state via liongard_launchpoint / liongard_metric and note dataprint age.
2. **History first.** search_tickets for this client + Sage/Pervasive/Actian: a recent Sage or PSQL upgrade (version mismatches between server and workstation engines are a classic break), a server IP/name change, a permissions/GPO change, or an antivirus change. And check the calendar — a break during close needs a faster, safer path and clear expectation-setting.
3. **Get the exact error and engine state before theorizing.** The verbatim error (Sage error text, or the PSQL status code — these are specific, e.g. status 3xxx/94/116 mean particular things), whether the Actian/Pervasive engine service is running on the host and in the right mode, and whether a workstation can reach the data path and the engine. Read the code/state — don't touch company data on a hunch.
4. **Branch:**
   1. **Data path / connection** — workstations can't find the company: the data path config points at a stale server name/IP or a mapped drive that isn't consistent, or the UNC path isn't reachable. Sage prefers a consistent path across all workstations — confirm the host resolves by name and the path matches the documented one. A host IP change (DHCP) breaks everyone — a reservation/static IP is the durable fix.
   2. **Pervasive/Actian engine** — engine errors or the service down: confirm the engine service is running on the host in **server** mode (workstations run a workgroup/client engine — a version or mode mismatch between them is a top cause), and that the server and workstation PSQL versions are compatible. Map the PSQL status code to Actian's documented meaning (web_search — don't guess). Escalate when: the engine won't start or reports database-level corruption — that can be an Actian/Sage vendor case, and company data integrity is at stake.
   3. **Permissions** — one user in, others out, or read-only behaviour: the share and NTFS permissions on the data folder must allow the users full control to the data (Sage writes lock/temp files there). A permissions or ownership change (or a new user not in the right group) causes exactly this. Fix the group/permission gap; don't loosen to Everyone-full to "make it work".
   4. **Multi-user / locking under load** — lock errors or slowness at close: heavy concurrent posting, an antivirus scanning the live data files, or a workstation that crashed holding a lock. Antivirus exclusions for the data folder and engine are the standard vendor recommendation — route an exclusion request through the security owner rather than disabling AV. A stuck lock usually clears once the offending session is confirmed closed.
5. **Verify and note.** Success is a second workstation opening the company in multi-user mode and posting a test action cleanly. Plain-text note: product/version, PSQL version/mode, data path, the error/status code, branch, action or handoff, verification — and note if this was during close.

## Guardrails

- **This is live accounting data, often mid-close** — warn before anything that risks data state, prefer scheduling disruptive steps outside posting windows, and make sure the client's finance lead knows before any downtime.
- No remote execution — engine, permissions, and connection-manager steps are guidance for a tech with access; use get_ninjaone_device_link to reach the host/workstation when the RMM integration is enabled. Restarting the Actian/Pervasive service via control_ninjaone_windows_service (when enabled) disconnects every Sage user — confirm no one is posting first.
- Never hand-edit or "repair" Sage company data files directly, and never delete lock/temp files while users are in the company — database corruption beyond Sage's own tools is a vendor case.
- Don't loosen share/NTFS permissions to Everyone-Full to work around an access gap — grant the intended group the needed rights.
- Don't disable antivirus to fix locking — request the vendor-recommended exclusions through the security owner.
- Do not invent PSQL status-code meanings, version-compat rules, or engine modes — web_search Actian/Sage current docs and cite.
- Docs/Liongard coverage varies per tenant — note what you couldn't check and dataprint age. Notes destined for a PSA sync: plain text, no markdown or emojis.
