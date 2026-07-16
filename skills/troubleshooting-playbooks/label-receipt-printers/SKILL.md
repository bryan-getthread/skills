---
name: Label and Receipt Printers
description: Diagnose thermal label and receipt printers — Zebra/thermal/POS units — where the print language (ZPL/EPL/ESC-POS) and driver mode, the spooler, and network/connection are the usual faults, distinct from office laser/MFP printing (printer-troubleshooting) and POS-app issues (pos-system-issues).
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Label and Receipt Printers

Thermal label and receipt printers fail differently from office printers: the output is driven by a **printer language** (ZPL/EPL for labels, ESC/POS for receipts), so a garbage-or-blank print is usually a language/driver mismatch, not a hardware fault. This playbook works the language/driver, spooler, connection, and media matrix. Office laser/MFP printing is printer-troubleshooting; a POS application that won't print is pos-system-issues (the app and the printer are different layers).

## When to use

- A Zebra/thermal label printer prints blank, garbage/code, misaligned, or wrong-sized labels
- A receipt printer at a POS/register won't print, prints gibberish, or double-prints
- Labels/receipts stopped after a driver, OS, or application change
- Barcode labels scan poorly, or the printer is offline/unreachable on the network

## Steps

1. **Model, language, and connection first.** search_itglue / search_hudu / search_knowledge_base for the device: make/model (Zebra, etc.), the **print language** it's set to (ZPL vs EPL for Zebra; ESC/POS for most receipt printers), how it's driven (the OEM driver, a generic-text/driver, or the application sending raw language directly to the port — many label/POS apps bypass the Windows driver entirely), the connection (USB / Ethernet-IP / Bluetooth / serial), and the media (label size, gap/black-mark/continuous, direct-thermal vs thermal-transfer ribbon). Language and drive-method are the fork — establish them first. If a Liongard/RMM inspector covers the host, corroborate spooler/queue state and note dataprint age.
2. **History first.** search_tickets for this client + this printer/label/receipt: a recent driver or OS update, an application update, a printer replacement (a new unit defaulting to a different language is a classic), a media/ribbon change, or an IP change. Sudden garbage output right after a swap usually means language/driver mismatch.
3. **Isolate the layer before theorizing.** Print the printer's own **self-test / configuration label** (from the device buttons) — if that's clean, the hardware and media are fine and the problem is upstream (driver/app/connection). Then test whether it's the app or the OS: does a Windows test page (or a known-good label from the OEM utility) work? Reading the config label tells you the printer's current language, DPI, and network settings directly. Isolate hardware vs driver vs app first.
4. **Branch:**
   1. **Language / driver mismatch** — prints raw code (ZPL/EPL text printing literally), blank, or wildly wrong: the driver or app is sending a language the printer isn't set to, or a raw-language app is being routed through a Windows driver that re-encodes it (or vice-versa). Match them: set the printer's language or the driver to agree, and for raw-sending apps use a generic/raw port path, not a translating driver. A printer replaced with one defaulting to the other language needs its language set or the driver changed.
   2. **Sizing / calibration / alignment** — labels come out shifted, skipping labels, or wrong length: the printer needs **media calibration** (so it learns the label gap/black-mark/length) and the driver/label template size must match the actual media. Recalibrate from the device and confirm the template dimensions. Poor barcode scanning is usually darkness/speed settings or worn printhead/media — adjust darkness or check the printhead.
   3. **Spooler / queue / connection** — offline, stuck queue, or nothing prints: check the queue and spooler on the host (a stuck job blocks the rest), the port (USB re-enumeration, the network printer's IP reachable and matching the port config — a DHCP change orphans the port), and for shared printers the print server. A network label/receipt printer that "disappeared" is usually an IP change — a reservation/static IP is the durable fix.
   4. **Media / hardware** — the self-test label itself is bad (faded, streaked, blank): direct-thermal media loaded upside down, a spent or mismatched ribbon (thermal-transfer needs ribbon; direct-thermal must not), a dirty or failing printhead, or the wrong media type. Faded output across the whole label is media/darkness/printhead. Escalate when: it's a failed printhead/hardware — that's a replacement/vendor path.
5. **Verify and note.** Success is the real output correct from the real workflow — a properly sized, scannable label or a clean receipt from the actual application, not just a test page. Plain-text note: model, language and drive-method, the isolation result (self-test clean?), branch, action or handoff, verification.

## Guardrails

- No remote execution — driver, spooler, calibration, and OEM-utility steps are guidance for a tech/user on the device; use get_ninjaone_device_link to reach the host when the RMM integration is enabled. Restarting the print spooler via control_ninjaone_windows_service (when enabled) affects everyone printing on that host — confirm impact.
- Separate the layers: a POS/label *application* that won't print is pos-system-issues; office laser/MFP is printer-troubleshooting; this playbook is the thermal device, its language, and its connection.
- Confirm the printer's own self-test/config label before chasing drivers — it cleanly divides hardware/media faults from upstream ones.
- Don't guess ZPL/EPL/ESC-POS commands or model-specific calibration sequences — web_search the OEM's current docs for that model and cite; settings differ by model and firmware.
- Failed printheads and worn hardware are a replacement/vendor path — don't keep tweaking darkness on a dying head.
- Docs/Liongard coverage varies per tenant — note what you couldn't check. Notes destined for a PSA sync: plain text, no markdown or emojis.
