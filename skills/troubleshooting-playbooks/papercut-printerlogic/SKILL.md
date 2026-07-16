---
name: PaperCut / PrinterLogic
description: Diagnose print-management platform problems — PaperCut and PrinterLogic (and similar) release stations, driver/printer deployment failures, and quota/account issues — from the platform's own logs and deployment state, not the raw print spooler alone. For plain spooler/driver faults use printer-troubleshooting.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# PaperCut / PrinterLogic

Print-management platforms add a control layer over printing — release stations, self-service driver deployment, and quota/charge accounting — and each of those breaks in its own way. This playbook works from the platform's server/agent logs and deployment state. A pure spooler, driver, or hardware fault (no management layer involved) belongs to printer-troubleshooting.

## When to use

- Held/secure-release jobs won't release at a release station or via the app/card reader
- A printer won't deploy to workstations (PrinterLogic self-service / PaperCut Print Deploy), or the wrong driver installs
- Users blocked by quota/balance, wrong account charged, or accounting not recording jobs
- The management client/agent isn't connecting to the server, or the print provider went offline

## Steps

1. **Platform and version first.** search_itglue / search_hudu / search_knowledge_base for the setup: which platform and version, the deployment model (on-prem PaperCut MF/NG with secondary print servers vs PrinterLogic SaaS/on-prem with the provider/agent), how printing is routed (direct-IP via the platform vs shared queues), the client/agent on workstations, release-station/card-reader hardware, and how identity maps (AD user → platform account). Confirm which layer the complaint touches — release, deploy, or accounting. If a Liongard Windows/print inspector runs, corroborate server/queue state via liongard_launchpoint / liongard_metric and note dataprint age.
2. **History first.** search_tickets for this client + printing/PaperCut/PrinterLogic: a recent platform upgrade, a print-server or driver change, a Windows print-spooler security update (Microsoft's print-nightmare-era updates have repeatedly broken third-party print deployment — check patch dates), or a card-reader/release-station change. Sudden fleet-wide onset after a patch is the classic tell.
3. **Get the platform's evidence before theorizing.** The platform's admin console and logs (PaperCut App Log / print-provider log; PrinterLogic admin portal + client logs) — the job's status in the platform, whether the client/agent is checked in, and the deployment/policy assignment for the affected user/printer. Separately confirm the underlying spooler is healthy on the server (a platform can't release through a dead spooler). Read the platform's own record of the job — not just the workstation.
4. **Branch:**
   1. **Release-station / secure release** — a held job never releases: check the job actually reached the server and is held (not failed at submit), the release station/app/card reader is online and mapped to the right printer, and the user authenticated (card→account mapping). A card reader that stopped authenticating is hardware/mapping, not the queue. Escalate when: the release/reader hardware is faulty — that's a hardware/vendor path.
   2. **Deployment / driver push** — a printer won't deploy or installs the wrong/broken driver: confirm the client/agent is installed and connected, the printer is assigned to that user/group/OU in the platform, and the driver package is valid for the OS. Post-Windows-print-update breakage often needs the vendor's compatible client version or a driver/package update — check the vendor's current guidance (web_search). A Type-3 vs Type-4 driver or point-and-print policy change frequently underlies "won't install".
   3. **Quota / accounting** — a user is blocked or the wrong account is charged: read the platform's account balance/quota rules and the job's charged account. Wrong-account charging is usually identity mapping (shared workstation, generic login) or a cost-center rule. Adjusting a balance/quota is a policy action — confirm with the account owner before changing charges; never zero out or grant quota to "unblock" without the client's cost policy in mind.
   4. **Client/agent or provider offline** — nothing works for a site or everyone: the print provider service (PaperCut) or the PrinterLogic provider/agent lost connection to the server/cloud, a secondary print server is down, or a firewall/cert change broke the client-server channel. Check the service/provider state and connectivity first — this masquerades as many individual failures.
5. **Verify and note.** Success is an end-to-end test: submit → hold → release a real job (or deploy the printer to a test user, or confirm the correct account charged). Plain-text note: platform/version, layer (release/deploy/accounting), the platform-log evidence, branch, action or handoff, verification.

## Guardrails

- No remote execution — admin console, client, and spooler steps are guidance for a tech with the right access; use get_ninjaone_device_link to reach a workstation/server when the RMM integration is enabled. Restarting the print spooler or the print-provider service via control_ninjaone_windows_service (when enabled) is disruptive to everyone printing — confirm impact and prefer off-hours.
- Quota/balance/charge changes are the client's money/policy — confirm with the account owner before adjusting; never grant or zero quota casually.
- Card-reader mappings and platform accounts tie to people — refer to users by placeholder, keep identity mappings out of PSA notes.
- After a Windows print-security update broke deployment, the fix is usually a vendor-supported client/driver version — verify against the vendor's current docs (web_search), don't roll back security patches as a reflex.
- Do not invent log locations, version behaviours, or driver-type rules — web_search the vendor's current docs and cite.
- Docs/Liongard coverage varies per tenant — note what you couldn't check and dataprint age. Notes destined for a PSA sync: plain text, no markdown or emojis.
