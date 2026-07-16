---
name: Scanner & Copier Fleet
description: Work MFP/copier tickets beyond printing — scan-to-folder failures after credential or SMB changes, address-book cleanup, firmware quirks, panel errors — with a clear line between what the desk fixes and what the lease vendor owns.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Scanner & Copier Fleet

Copiers are embedded computers that age in place: they hold service-account passwords that someone eventually rotates, speak SMB dialects that servers eventually deprecate, and run firmware nobody updates until scanning breaks. This playbook covers the scan/fleet side; for print-path and scan-to-email symptoms, pair with the Printer Troubleshooting playbook.

## When to use

- "Scan to folder stopped working" — often the whole office, often right after a password change or server work
- "The copier can't reach the share anymore" after a server migration or security hardening
- Address book entries wrong/missing, or a request to add/change scan destinations
- Panel error codes, firmware-related weirdness, or "the copier company says it's a network problem"

## Steps

1. **History first.** search_tickets for this client + the copier/MFP. Scan-to-folder tickets cluster after identifiable events — a password rotation, an SMB1 disablement, a file-server migration — and the ticket history usually names the event.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the fleet profile: copier models and firmware, the scan service account, destination share paths, the lease/service vendor and what their contract covers, admin-panel access details location (never the credentials themselves in notes).
3. **Establish the boundary early.** Hardware faults, consumables, feed/jam mechanisms, and often firmware updates belong to the lease vendor's service contract. Network, credentials, shares, and DNS belong to the desk. Say which side a ticket falls on before deep-diving.
4. **Identify versions.** Copier model and firmware version, and — critical — the SMB dialect the destination server accepts. Older copier firmware speaks only SMB1; servers hardened to SMB2+ silently orphan them. Never assume; check docs or ask what changed server-side.
5. **Evidence before theory.** The copier's own error (panel message, send-log entry, error code — web_search the code with the model, do not guess), plus scope: one destination failing vs all destinations vs all copiers.
6. **Branch:**
   1. **Authentication** — scans fail everywhere after a credential change. The copier embeds a service account for SMB writes; if that account's password rotated or the account was disabled in a cleanup, every destination dies at once. Guide: confirm the account's state, then update the copier's stored credential via the admin panel. The durable fix is a documented, non-expiring-by-accident service account with least privilege on the scan shares — flag any password policy collision to whoever owns identity. Escalate when: the client wants scans authenticated as individual users — that is a design change, not a fix.
   2. **SMB version / protocol** — scans fail after server hardening or migration; credentials verified good. If the firmware maxes out at SMB1, the honest options are a vendor firmware update (lease vendor's job — never re-enable SMB1 on the server as the "fix"; that reopens a known attack surface and is a security decision, not a convenience) or an intermediary path the client accepts. Escalate when: firmware update needed — lease vendor; SMB1 re-enable requested — security owner sign-off required, in writing.
   3. **Destination path** — one destination fails, others work. The share moved, was renamed, or its permissions changed. Verify the path exists and the scan account has write access; fix the copier's destination entry to match reality. Escalate when: the underlying share permissions are the problem — ladder them with the File Share Permissions playbook.
   4. **Address book** — wrong/missing entries, or bulk changes requested. Small edits via admin panel guidance; for bulk changes, most vendors support export/import — get the vendor's procedure via web_search for the model. Keep a copy of the export in client docs before importing. Escalate when: the address book should sync from a directory — vendor-specific feature, lease vendor or vendor docs decide feasibility.
   5. **Firmware quirks / panel errors** — reboots, panel error codes, features that vanished. Match the code to the vendor's published meaning. Firmware updates on leased devices are almost always the lease vendor's to perform. Escalate when: any panel error code that maps to a hardware subsystem — straight to the lease vendor with the code and model.
7. **Close the loop.** Have the user run a real scan to the affected destination before resolving. Post a plain-text note: model/firmware, error/code, branch, what changed, who owns any remaining work (lease vendor items explicitly), verification result.

## Guardrails

- No script or remote execution — remediation is admin-panel guidance for the tech or a lease-vendor handoff.
- Never re-enable SMB1 or weaken server security to accommodate old firmware without explicit, recorded sign-off from the security owner — offer the vendor-firmware path first and say why.
- Never place credentials in ticket notes; reference where they are stored, not what they are.
- Respect the lease boundary: touching hardware or vendor-controlled firmware can void the service contract — route it and say so.
- Do not invent error-code meanings or vendor procedures — web_search with the exact model and cite. search_itglue / search_hudu may be absent for this tenant; fall back to search_knowledge_base and say what you could not check.
- Notes destined for a PSA sync: plain text, no markdown or emojis.
