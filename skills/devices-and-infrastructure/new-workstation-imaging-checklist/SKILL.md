---
name: New Workstation Imaging Checklist
description: Run the standard build-and-deploy checklist for a new or re-imaged workstation — naming, OS baseline, enrollment, apps, profile, verification, and handoff. Use when someone asks to image a machine, prep a new laptop, or "get this workstation ready for <user>".
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_link, search_itglue, search_knowledge_base, create_ticket, add_ticket_note]
connectors: [NinjaOne, IT Glue]
---

# New Workstation Imaging Checklist

**When to use:** "Set up this new laptop for <user>", a new-hire ticket needs a machine built, or re-imaging a returned/repaired device before redeployment.

## Prompt

```
Drive a new or re-imaged workstation from bare hardware to user-ready against a consistent build standard, with a single verification trail. This skill produces a checklist, verification, and handoff — it does not push software, run scripts, or deploy policy through the RMM.

1. Establish the target: client, assigned user (if any), device model, and whether this is a fresh build or re-image. Pull the client's build standard from documentation (search_itglue) and any KB build guide (search_knowledge_base); if none exists, say so and use the generic checklist below rather than inventing client-specific steps.
2. Confirm the deployment method before starting. For modern Windows fleets the path is usually Autopilot + Intune — hand that portion to the m365-administration autopilot/intune skills rather than duplicating it. Note in the ticket which method applies (Autopilot/Intune, manual image, or MDM for Mac).
3. Walk the build checklist and record each item as done / N/A / blocked:
   - Naming: hostname per the client's convention; asset tag recorded.
   - OS baseline: current OS build, firmware/BIOS updated, disk encryption enabled (BitLocker/FileVault) with the recovery key escrowed to the correct store.
   - Management: RMM agent installed and checking in; enrollment (Autopilot/Intune or MDM) completed; endpoint security/EDR agent installed.
   - Identity: joined to the correct directory (Entra/AD) and the right groups; user profile signed in and synced.
   - Apps: baseline app set plus any role-specific apps from the client standard.
   - Data: user data/profile migration if this is a replacement; verify before wiping any source device.
   - Peripherals/printers/drive mappings per the user's role and site.
4. Verify the device is actually managed: search_ninjaone_devices for the hostname, then get_ninjaone_device to confirm it is checking in, encrypted, and shows the expected agents (do not trust node_class — confirm in details). Reconcile the RMM record against documentation so the asset is tracked, not orphaned.
5. Update the asset record in documentation (owner, serial, warranty, deploy date).
6. Output a completion summary: checklist with each item's state, any blocked items with the reason, the device link (get_ninjaone_device_link) for the tech, and remaining handoff steps. Offer to post it as a plain-text note (add_ticket_note, no markdown/emojis), or create_ticket if this build has no ticket yet.

Guardrails: never wipe or re-image a source device until data migration is confirmed complete and verified. Do not fabricate a client build standard; if documentation has none, state that and use the generic baseline. Confirm the assigned user and site before joining directory groups or mapping resources — wrong-group placement is an access issue. If NinjaOne or documentation is absent, degrade to the checklist only and say the verification step could not be completed.
```
