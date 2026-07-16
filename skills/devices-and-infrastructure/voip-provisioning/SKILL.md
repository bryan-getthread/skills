---
name: VoIP Provisioning
description: Drive the configuration-and-onboarding workflow for a new VoIP phone / DID / extension — gather the provisioning details, follow the standard build, and verify — without touching live call routing. Use when someone asks to provision a phone, add an extension/DID, or onboard a handset.
category: Devices & Infrastructure
tools: [search_itglue, search_knowledge_base, search_ninjaone_devices, create_ticket, add_ticket_note]
---

# VoIP Provisioning

Provision a VoIP handset or extension consistently: capture what's needed, follow the client's phone-system build standard, and verify the device registers — a configuration workflow, not telephony control.

## When to use

- "Set up a new desk phone / extension for <user>."
- Adding or assigning a DID to a user or site.
- Onboarding a batch of handsets for a new site or team.
- Standardizing an ad-hoc phone setup against the client's PBX/UCaaS config.

## Steps

1. Identify the phone system and its build standard. Confirm the platform (hosted UCaaS or on-prem PBX) and pull the client's provisioning guide and numbering/extension plan from documentation (`search_itglue`) and any KB build guide (`search_knowledge_base`). If no standard exists, say so and capture requirements generically rather than inventing a dial plan.
2. Gather the provisioning details — mark each captured / missing:
   - User/assignment: user name, extension, DID (if any), site, and role (reception, hunt-group member, hot-desk).
   - Device: handset model, MAC address, firmware, and whether it's physical, softphone, or DECT.
   - Config: caller-ID name/number, voicemail-to-email, call-handling/hunt-group/ring-group membership, time-of-day routing profile it belongs to, e911/emergency address for the site.
   - Network: VLAN/QoS and PoE availability at the drop (cross-reference switch-vlan-change for the port config).
3. Walk the build checklist and record each step: create/assign the extension and DID in the platform, apply the handset template/profile, set caller ID and voicemail, add to the correct ring/hunt groups, and register the device.
4. Verify: confirm the handset registers to the platform and can place/receive a test call, voicemail delivers, and the emergency-address/e911 mapping is correct for the site. Where the phone is a network device visible to the RMM, confirm it's checking in (`search_ninjaone_devices`).
5. Output a provisioning summary: assignment, device, config applied, verification results, and any missing inputs. Update the asset/extension record in documentation. Post to the ticket as a plain-text note (`add_ticket_note`) or `create_ticket` for the request.

## Guardrails

- This is device/extension configuration only. It does NOT dial, route live calls, port numbers, or otherwise control telephony — those are carrier/platform actions outside the agent's reach. Do not represent call routing as something this skill performs.
- Emergency-address/e911 mapping must be verified for the device's actual physical location — never leave it defaulted or assumed.
- Do not invent DIDs, extensions, or a dial plan; mark unknowns as missing.
- Configuration is applied by the tech in the phone-system console; this skill gathers, guides, and verifies. Do not claim to have changed platform config you cannot confirm.
- Notes posted to tickets are plain text — no markdown, no emojis.
