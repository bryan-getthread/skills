---
name: IIS Web App
description: Diagnose IIS-hosted web application failures — app pool crashes and rapid-fail protection, binding/SSL problems, HTTP 500/502/503 decoding, and recycling behaviour — from the app pool state, HTTP.sys/FREB logs, and the exact sub-status, never from the browser error alone.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# IIS Web App

An IIS error page tells you almost nothing until you read the sub-status and the app pool state. This playbook decodes the exact status/sub-status (500.x, 502.x, 503), checks whether the app pool is even running, and reads the logs — before anyone recycles a pool or reinstalls anything.

## When to use

- An internal or client-facing site returns 500, 502.x, or 503 (Service Unavailable)
- An app pool keeps stopping ("rapid-fail protection" disabled the pool) or recycles constantly
- HTTPS binding broke after a certificate renewal, or the wrong cert is served
- A site works on the server but not remotely, or one app under a site fails while others work

## Steps

1. **Version and app first.** search_itglue / search_hudu / search_knowledge_base for the server: Windows/IIS version, whether this hosts a **vendor's** web application (many LOB apps ship an IIS front end — see the vendor-caution guardrail), the app pool → site → app mapping, the .NET/runtime version and pipeline mode, the service account the pool runs as, and the bindings/certs expected. If a Liongard IIS/Windows inspector runs, read site/binding/cert state via liongard_launchpoint / liongard_metric and note dataprint age.
2. **History first.** search_tickets for this client + the site: a recent deployment, a certificate renewal (503/binding breaks track cert expiry), a Windows/.NET patch, or a service-account password change (a pool whose identity password changed won't start). Sudden onset after a change names the cause.
3. **Get the exact status and pool state before theorizing.** The precise **status and sub-status** (500.19 config vs 500.21 handler vs 502.5 process-failure vs 503 pool-stopped are entirely different problems — the browser's generic "500" is useless), whether the app pool is Started or Stopped, the System event log for WAS/rapid-fail entries, the application event log for the app's own exception, and IIS/HTTP.sys logs. Enable Failed Request Tracing (FREB) for an intermittent 500 if needed. Read the sub-status — do not act on the browser page.
4. **Branch:**
   1. **App pool crashing / rapid-fail protection** — the pool is Stopped and WAS logged that it was disabled after N failures in the rapid-fail window. That means the worker process (w3wp) is dying — read the application event log / crash dump for the real exception (a bad deployment, a missing dependency, a runtime version mismatch, memory). Restarting the pool without fixing the crash just re-trips protection. Escalate when: the crashing code is a vendor app's — package the exception for the vendor.
   2. **Bindings / SSL** — HTTPS fails, wrong cert served, or 503/handshake errors after a renewal. Check the site binding maps to the correct (valid, in-store) certificate — a renewed cert with a new thumbprint often isn't re-bound; SNI vs IP bindings and hostname/port collisions also cause this. Pair with ssl-certificate-renewal. Verify the HTTP.sys SSL binding, not just the IIS UI, for stubborn cases.
   3. **500-series decode** — 500.19 = configuration/web.config (bad section, locked config, permissions on the file); 500.21/500.19 handler = a runtime/module not registered (e.g. ASP.NET/handler not installed); 500.0/502.5 = the app or runtime process itself failing to start. Map the exact sub-status to its documented meaning (web_search — don't guess) and fix that specific layer.
   4. **503 Service Unavailable** — almost always the app pool is stopped/disabled (see branch 1) or HTTP.sys has no listener for the binding. Confirm the pool state first; 503 is rarely an app-code problem and usually an infrastructure/pool one.
   5. **Recycling behaviour** — a pool recycling constantly (scheduled interval, memory limits, or config-change triggers) drops sessions and warms slowly. Read the recycle events and decide whether the trigger is intended (nightly recycle) or a memory leak forcing it. Tuning recycle settings on a vendor app is the vendor's guidance, not a guess.
5. **Verify and note.** Success is the exact failing request returning 200 (or the app's real page), the pool Started and staying up, and the correct cert served. Plain-text note: status/sub-status, pool state, the log evidence, branch, action or handoff, verification.

## Guardrails

- **Vendor web-app caution:** if IIS is hosting a vendor's application, do not edit its web.config, change its app-pool runtime/identity, or "fix" its code path without the vendor's guidance — you can break support and the app. Read and diagnose; hand config/code changes to the vendor or the client's app owner.
- No remote execution — appcmd/IIS Manager/PowerShell steps are guidance for a tech with server access; use get_ninjaone_device_link to reach the host when the RMM integration is enabled. Restarting the W3SVC/WAS service or recycling a pool via control_ninjaone_windows_service (when enabled) is a disruptive action — confirm impact and prefer off-hours.
- Never resolve a 503 by just re-enabling a pool that rapid-fail disabled — that hides a crashing app; find the exception first.
- Decode the exact sub-status; do not invent status meanings, web.config sections, or handler names — web_search and cite.
- Certificate/binding fixes touch live HTTPS — pair with ssl-certificate-renewal and treat as a change.
- Docs/Liongard coverage varies per tenant — note what you couldn't check and dataprint age. Notes destined for a PSA sync: plain text, no markdown or emojis.
