---
name: Hardware Diagnostics
description: Work suspected hardware faults on desktops and laptops — no-boot, random shutdowns, disk noises, battery/thermal complaints — through the POST/boot-stage ladder, storage and battery health reads, and warranty routing.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, get_ninjaone_device, get_ninjaone_device_activities, get_ninjaone_device_link, add_ticket_note, web_search]
---

# Hardware Diagnostics

Hardware tickets reward discipline: identify *where in the power-on sequence* the machine fails, read the health data the machine already keeps (SMART, battery, thermals), and route to warranty with the vendor's required evidence instead of parts-cannon guessing. Software playbooks handle the OS; this one owns the metal.

## When to use

- "<user>'s machine won't turn on / won't boot" — any stage of dead
- Random shutdowns, freezes under load, fan roar, or burning-hot laptop
- Clicking/grinding disk, "SMART error" messages, or suspicion of failing storage
- Battery dies in an hour; deciding repair vs warranty vs replace

## Steps

1. **History first.** search_tickets for this device — prior thermal/disk/crash tickets make today's symptom a progression, not a debut, and change the answer toward replacement. When NinjaOne is enabled: get_ninjaone_device for model, age, disk health, and last-seen; get_ninjaone_device_activities for recent alerts and the shutdown/crash pattern.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the device's record: purchase date, warranty status/vendor, and the client's refresh policy — a machine past refresh age gets minimal diagnosis and a replacement recommendation, and say so early.
3. **Identify the machine precisely.** Vendor, model, and serial/service tag (from the chassis, firmware screen, or RMM). The serial drives everything downstream: vendor diagnostics, warranty lookup, and the support case.
4. **Ladder the power-on sequence — where it stops is the diagnosis:**
   1. **No power** (no lights, no fans) — power path: outlet/strip verified, the *correct* adapter genuinely seated both ends (laptop adapters fail more than laptops — swap a known-good one before anything), battery-only vs adapter-only behavior, and the vendor's documented power-drain/reset procedure for the model (verify with web_search). Desktop: PSU switch/cable, then PSU itself.
   2. **Power but no POST** (fans/lights, no display, or beep/blink codes) — capture the beep pattern or diagnostic-LED code exactly and decode it against the vendor's documentation for this model — never generically (codes differ per vendor/line). The code names the subsystem (memory, board, GPU). Reseat what the vendor's guidance allows (RAM is the classic); board-level faults → warranty branch.
   3. **POSTs but no boot device / boot loops** — firmware sees the disk? If absent/intermittent → storage branch (5). Present but won't boot → the line between hardware and OS: run the vendor's built-in hardware diagnostics (every major vendor ships one at boot) *before* OS repair, so a dying disk doesn't eat an afternoon of OS surgery.
   4. **Boots but misbehaves under load** (shutdowns, freezes, throttling) — thermal/power branch (6) or memory (vendor diagnostic's memory test; varying BSOD codes corroborate — pair with the BSOD playbook).
5. **Storage health.** Read SMART status (RMM surfaces it when enabled; otherwise the vendor diagnostic or OS tools). Any pre-failure indicator, reallocated-sector growth, or audible clicking = treat as failing *now*: stop diagnostics that stress the disk, verify backup/sync state immediately, image or copy data off per the client's practice, then replace. Data first, hardware second — always in that order, and say so to the user.
6. **Battery and thermals.** Battery: generate/read the platform's battery health report (design vs full-charge capacity, cycle count); below the vendor's service threshold or swelling (any deformation = stop using the device immediately, do not charge it — safety first) → battery replacement via warranty or parts. Thermals: temperatures at idle vs load, fan behavior, vent condition — dust and blocked vents fix half of these; repeated thermal shutdowns after cleaning → cooling-system fault, warranty/repair branch.
7. **Warranty routing — with the evidence the vendor demands.** Look up warranty by serial on the vendor's site (web_search / the documented portal). In warranty: open the case with what vendors require — serial, the diagnostic code or the vendor-diagnostic's failure ID (the onboard diagnostics emit an error code precisely for this), symptom description, troubleshooting already done. Capture the case number in the ticket. Out of warranty: cost the repair against the client's refresh policy and the device's age, and recommend honestly — an out-of-warranty board replacement on a five-year-old laptop is rarely defensible; write the numbers.
8. **Verify and note.** Post-repair: re-run the vendor diagnostic clean, and the user works a normal day without recurrence. Plain-text note: model/serial, ladder stage reached, codes/health data captured, data-preservation status, branch, warranty case number if opened, verification.

## Guardrails

- Data before hardware: at any failing-storage signal, backup/imaging comes before further diagnostics or repair — a warranty replacement doesn't return the client's files, and say so plainly.
- Decode beep/LED/diagnostic codes only against the vendor's documentation for that model via web_search — never from generic memory; wrong decode sends the wrong part.
- Swollen batteries are a safety stop: cease use and charging immediately, handle per the vendor's guidance — no troubleshooting continues on that device.
- Physical repairs follow the vendor's service procedures and warranty terms — opening what the warranty says not to open voids it; when the vendor's depot/onsite is the entitled path, route there instead of bench heroics, and say when only the vendor can act.
- No remote execution — RMM reads inform, but hands-on steps are the tech's or user's; use get_ninjaone_device_link for the handoff when enabled, and note when RMM/docs integrations are absent for the tenant.
- Recommend replace-over-repair with numbers (age, refresh policy, repair quote) — not vibes; the client decides with the evidence in front of them.
