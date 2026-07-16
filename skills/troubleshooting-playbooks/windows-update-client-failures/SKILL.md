---
name: Windows Update Client Failures
description: Diagnose a single machine failing Windows updates — 0x8024xxxx/0x800Fxxxx errors, updates that loop or roll back, "checking for updates" forever — by first identifying which update source the machine actually reports to (WU, WSUS, or Intune), then walking the component-store repair ladder in order.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Windows Update Client Failures

Two questions decide most of these tickets: (1) where does this machine think updates come from, and (2) what is the exact error code. Skipping to "reset Windows Update components" without those wastes the ticket. Fleet-wide failures point at the infrastructure — that's the wsus-patching-infrastructure playbook, not this one.

## When to use

- One machine (or a handful) erroring on updates with an 0x8024xxxx / 0x800Fxxxx / 0xC1900xxx code
- An update installs, reboots, then rolls back ("We couldn't complete the updates")
- "Checking for updates" hangs, or a machine shows compliant in one console and stale in another
- RMM patch reports flag a device repeatedly failing the same KB

## Steps

1. **History first.** search_tickets for this device and this client for the same KB/error. Same KB failing across many machines → known-bad update or infrastructure (WSUS approval/content) — check the vendor's known-issues page (web_search) and consider wsus-patching-infrastructure before touching individual machines. One machine → continue here.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the client's patch design: WSUS server? Intune update rings? RMM-driven patching (pair with the RMM's own status rather than fighting it)? Dual scan/co-management history?
3. **Identify the update source on the machine — do not assume.** Guide the tech: `Get-WindowsUpdateLog` is heavy; faster truth is the registry/policy view — `HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` (WUServer set = WSUS), MDM diagnostics / `dsregcmd /status` for Intune enrollment, and Settings → Windows Update → the "managed by your organization" banner. **Update-source confusion is a first-class root cause:** a machine pointed at a decommissioned WSUS never updates and never errors loudly; a machine with both WSUS policy and Intune rings can scan against the wrong source. If the source is stale/conflicting, fix the pointing (per client design) before any repair ladder.
4. **Get the code before theorizing.** Exact error from Settings/Update history or CBS. Decode by family, verifying the specific code via web_search rather than memory:
   - **0x8024xxxx (Windows Update agent)** — commonly transport/source: can't reach the source, WSUS content missing, proxy/TLS interception. Test reachability to the actual source found in step 3.
   - **0x800F0831 / 0x800F09xx / 0x800F081F (servicing/CBS)** — component store corruption or a missing servicing chain payload → the repair ladder below.
   - **0x80070070-family (disk full)** — check free space before anything clever; feature updates need tens of GB.
   - **0xC1900xxx (feature-update/setup)** — driver/app compatibility; read the SetupDiag output rather than guessing.
5. **The component-store repair ladder — in order, no skipping:**
   1. `DISM /Online /Cleanup-Image /ScanHealth` then `/RestoreHealth` (note: on WSUS-managed machines RestoreHealth may need `/Source` or GPO "repair from Windows Update" allowed — a RestoreHealth 0x800f081f on a WSUS client is usually source, not corruption).
   2. `sfc /scannow` **after** DISM (SFC repairs *from* the store DISM just fixed — running SFC first can "repair" from a corrupt store).
   3. Retry the update. Only if still failing: rename SoftwareDistribution/Catroot2 (the "reset components" step — safe but destroys update history and re-downloads everything; do it once, deliberately, not as a ritual).
   4. Still failing → in-place upgrade repair (keeps apps/files) is the honest next step; schedule it with the user rather than looping the ladder.
6. **Rollback loops (installs then reverts).** Find the failing component: SetupDiag for feature updates, CBS log around the reboot for cumulative ones. Common causes: a filter driver (AV/backup agent) or bad third-party driver. Pausing the *specific* update while the vendor issue is confirmed is legitimate; disabling updates wholesale is not.
7. **Verify and note.** Success = the previously failing KB shows Installed in update history and the compliance console the client actually uses reflects it (allow a check-in cycle). Plain-text note: update source found, error code verbatim, ladder steps run and results, action or handoff, verification and time.

## Guardrails

- No script execution from here — DISM/SFC/registry checks are guidance for the tech or a deep-link handoff to the RMM tech console. Registry edits only under the client's documented patch design.
- Never disable Windows Update, block updates wholesale, or set indefinite deferrals to close a ticket — pause the specific problem update, note the re-review date.
- Don't "reset Windows Update components" as step one — it destroys the evidence (update history) the diagnosis needs.
- Machines failing because they point at dead infrastructure get fixed by re-pointing per client design — confirm the design with the client's patch owner, don't invent one.
- Decode error codes against Microsoft's current documentation (web_search), not memory; codes are reused across contexts.
- Many machines failing the same way = infrastructure. Stop and switch to wsus-patching-infrastructure (or the Intune/RMM equivalent conversation).
- Docs coverage varies per tenant — note anything you could not check.
