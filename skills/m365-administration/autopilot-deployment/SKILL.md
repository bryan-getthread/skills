---
name: Autopilot Deployment
description: Work Windows Autopilot requests end-to-end — hardware hash registration, profile assignment, ESP behavior — and make the reset-vs-re-enroll call when a deployment goes sideways. Use for "set this device up with Autopilot", "Autopilot is stuck", or "white glove / ESP hanging" tickets.
category: M365 Administration
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, send_approval, web_search]
---

# Autopilot Deployment

Autopilot fails in three distinct phases — registration (the hash), targeting
(the profile), and provisioning (the ESP) — and the fix is different in each.
This skill diagnoses by phase and keeps "just wipe it" from becoming the
default answer.

## When to use

- "Register these new laptops for Autopilot" / "import the hardware hashes."
- Device boots to a normal OOBE instead of the company-branded experience.
- Enrollment Status Page stuck or timing out during setup.
- "Do we reset this device or re-enroll it?" after a failed deployment.

## Steps

1. **Context.** search_itglue / search_hudu / search_knowledge_base for the
   client's Autopilot standard: profile settings (user-driven vs self-deploying,
   join type), device group logic, ESP configuration, and who supplies hashes
   (OEM/distributor registration vs manual capture).
2. **Registration phase.** For new devices: prefer OEM/partner registration at
   purchase. For manual capture, the tech collects the hardware hash
   (Get-WindowsAutopilotInfo or the HWID CSV path documented for the client)
   and imports it in Intune → Devices → Enrollment → Windows Autopilot.
   After import, confirm the device appears and — critically — reaches profile
   status "Assigned" before shipping/handoff. Import and assignment are not
   instant; do not declare a device ready while status is "Pending."
3. **Targeting phase.** If a device skips the Autopilot experience: verify the
   hash is registered (serial lookup), the device landed in the Autopilot
   device group (dynamic membership on the ZTDId tag takes time to evaluate),
   and exactly one deployment profile wins for that group. Overlapping profiles
   with different join types produce "worked for one batch, not the next."
4. **ESP phase.** Set expectations honestly: the ESP runs device setup then
   account setup, and it blocks on the apps and policies it is told to block
   on. If ESP hangs: identify which phase and which app from the device (ESP
   details) or Intune troubleshooting pane. Usual causes: a required app that
   fails or is very large, a Win32 app + line-of-business app mix issue, or
   ESP timeout set shorter than the real install time. Fix the blocking app or
   trim the ESP's blocking-app list via the client's change process — do not
   tell the user to "just click continue anyway" if the profile allows reset
   on failure.
5. **Reset vs re-enroll decision.** When a deployment is broken:
   - Config/app issue, device otherwise healthy → fix targeting, then sync or
     re-run; no reset.
   - Provisioning half-completed, no user data on device → Wipe with
     "re-provision" intent (Autopilot Reset for a device staying with the same
     tenant/user) — fastest clean redo.
   - Device previously used, user data present → data-loss warning applies:
     route through the device-wipe-workflows skill and its approval gate.
   - Hash never registered or registered to the wrong tenant → re-enrollment
     alone will not fix it; correct the registration first (deregister/import).
6. **Verify and note.** Success = device completes ESP, user signs in, required
   apps present, device compliant. Post a plain-text note: phase diagnosed,
   evidence, actions, hash/serial handled (serial only — never post the full
   hash blob into notes), and verification.

## Guardrails

- Any reset or wipe of a device that may hold user data goes through
  send_approval with the data-loss consequence stated plainly — an Autopilot
  redo is not exempt from the destructive-action gate.
- Never deregister an Autopilot hash as cleanup without confirming the device
  is not about to be redeployed — deregistration plus device deletion is how
  machines fall out of management permanently.
- Hardware hashes and CSV exports are sensitive device identity material: keep
  them out of ticket notes and email; reference by serial number.
- Do not lower ESP blocking requirements tenant-wide to fix one device — that
  changes the provisioning guarantee for every future deployment and needs
  client sign-off.
- Agent prepares the plan and decision; the technician executes all Intune
  console actions. Never mark registration or reset as done on intention.
