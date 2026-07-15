---
name: Printer Troubleshooting
description: Work a printing problem — nothing prints, stuck queue, garbled output, wrong printer, or scan-to-email failure — through a spooler/driver/network diagnostic matrix.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Printer Troubleshooting

Walks a print issue from symptom to a specific branch (spooler, driver, path, network, scan-to-email) with remediation guidance for the tech and explicit escalation points. Nothing here executes on the device — all remediation is guidance to relay or hand off.

## When to use

- "<user> can't print" / "the print queue is stuck for everyone"
- Garbled or half-printed pages, wrong tray, jobs vanish silently
- "Scan to email stopped working on the copier"
- One printer failing vs a whole office not printing

## Steps

1. **History first.** search_tickets for the same client + printer or same symptom. A recent identical ticket may hold the fix; multiple recent tickets suggest a shared cause (server, firmware, network), not the endpoint.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base (whichever this tenant has) for the client's print environment: print-server name, direct-IP vs shared printers, driver standard, copier SMTP settings.
3. **Scope the blast radius.** One user → endpoint branch. All users of one printer → device/queue branch. All printers → server/network branch. Ask if unclear; never assume.
4. **Identify versions.** Windows/macOS version, printer model, driver type in use (Type 3 kernel-mode, Type 4, or a Universal Print Driver). Never assume — ask or check docs.
5. **Get the evidence before theorizing.** Error text on the printer panel, the job status in the queue, PrintService event log entries (Admin + Operational), or the copier's scan-to-email error report.
6. **Branch:**
   1. **Spooler / stuck queue** — jobs pile up, "error - printing". Guide the tech: stop Print Spooler service, clear `%SystemRoot%\System32\spool\PRINTERS`, restart spooler. If it recurs within days → suspect a corrupt driver or a specific document type; move to branch 2. Escalate when: spooler crashes repeatedly with faulting module pointing at a driver DLL.
   2. **Driver** — garbled output, one app fails, crashes on print. Check driver type: Type 3 vs Type 4 mismatches on shared printers are a classic cause. Guide: remove and re-add the printer with the vendor's current driver, or standardize on the client's documented UPD. Escalate when: driver has a known defect only fixable by a vendor release — say plainly the vendor must ship the fix; a workaround (older driver, different driver type) is the interim.
   3. **Print-server vs endpoint path** — shared printer fails but direct-IP works (or vice versa). Guide the tech to test the other path to isolate. Server-side symptoms (all users of shared queues) → check the server's spooler, disk space, and recent Windows updates known to break printing (web_search the KB number before blaming it). Escalate when: server-wide outage — treat as an incident, not a per-user ticket.
   4. **Network path** — printer offline / intermittent. Guide: verify the printer's IP hasn't changed (DHCP vs documented static/reservation), ping it, test port 9100. If the IP drifted, the fix is a reservation, not a one-time re-point. Escalate when: printer drops off the network on a schedule → suspect switch port, power saving, or DHCP scope issues; route to the network resource.
   5. **Scan-to-email** — copier can print but scans fail. Get the copier's error code first. Check the documented send path: direct to M365 (basic SMTP auth is deprecated — devices need an approved relay method), an SMTP relay/connector, or a third-party relay service. Guide correction to match the documented working pattern for this client. Escalate when: the tenant's mail path itself changed (new gateway, disabled connector) — pair with the mail-flow playbook.
7. **Close the loop.** Have the user print a test page (or send a test scan) before resolving. Post a plain-text note: symptom, branch taken, evidence (error/event IDs), fix or handoff, verification result.

## Guardrails

- No script or remote execution exists — every remediation is instructions for the tech or the user, or a deep-link handoff into the RMM when that integration is enabled for the tenant.
- Never guess an IP, server name, or driver version — pull it from client docs or ask. Do not invent KB numbers or vendor links; verify with web_search.
- If only the vendor can fix it (firmware defect, driver bug), say so explicitly and offer the interim workaround.
- search_itglue / search_hudu may not be enabled for this tenant — fall back to search_knowledge_base and say what you could not check.
- Notes destined for a PSA sync: plain text, no markdown or emojis.
