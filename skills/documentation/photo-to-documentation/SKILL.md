---
name: Photo To Documentation
description: Turn rack, whiteboard, and label photos into structured documentation — transcribe exactly what's legible, flag what isn't, and double-check every serial number — so a phone photo becomes a searchable record instead of a lost attachment.
category: Documentation
tools: [add_ticket_note, search_itglue, search_hudu]
---

# Photo To Documentation

Converts the photos techs take on site — rack fronts, patch panels, whiteboard diagrams, device labels — into structured, searchable documentation under one rule: transcribe verbatim or flag as illegible, never "probably."

## When to use

- A tech pastes rack/label/whiteboard photos: "turn these into documentation."
- After a site visit (see documentation/network-diagram-request) the photo pile needs to become records.
- A device label photo needs its model and serial extracted into the asset entry.

For screenshots of on-screen text (error dialogs, emails), use communication/screenshot-ocr-translate — this skill is for photos of physical things.

## Steps

1. Classify each photo and apply the matching structure:
   - **Rack photo (front/back):** top-to-bottom inventory — rack unit position, device make/model as printed, visible label text, cable/port observations only where actually traceable in the image.
   - **Device label:** make, model number, serial number, MAC, service tag, port/asset stickers — each field exactly as printed.
   - **Patch panel:** port → label text mapping, in panel order; unlabeled ports recorded as UNLABELED.
   - **Whiteboard/paper diagram:** nodes and connections as drawn, text as written — including question marks and crossed-out items, which are information. Structure it as a list of nodes + a list of connections; do not "improve" the topology.
2. **Transcribe-verbatim-or-flag (the rule that makes this trustworthy):** every character either matches the photo exactly or the field is marked **[ILLEGIBLE]** / **[PARTIALLY LEGIBLE: <what is readable>]**. No completing a serial from the visible prefix, no inferring a model from the device's shape, no normalizing label spelling. A flagged gap gets re-photographed on the next visit; a silently guessed value corrupts the record for years.
3. **Serial numbers get a second pass:** re-read every serial/MAC/service-tag character-by-character against the image — the ambiguous glyphs (0/O, 1/I/l, 5/S, 8/B, 2/Z) decide warranty lookups and RMAs. If any character is uncertain after the second pass, transcribe with the uncertainty marked ("S/N: 7X4[0 or O]92…") rather than picking one.
4. Cross-check against existing records where doc platforms are enabled: search_itglue / search_hudu for the device/site — if the photo contradicts the record (different serial, moved rack position), report the discrepancy explicitly; the photo is evidence, and the human decides which record is stale.
5. Output the structured transcription per photo, each with: capture date/site if provided, the transcription, the flagged-illegible list, and the discrepancy list. If asked, file it to the visit ticket via add_ticket_note (plain text per the Note Format Standard skill, documentation/note-format-standard) and recommend the destination doc entry — writing to the doc platform is the human's step.

## Guardrails

- **Verbatim or flagged — never inferred.** The transcription's value is that it can be trusted blind; one plausible guess breaks that for the whole document.
- Serial numbers, MACs, and service tags are always double-checked (step 3), and uncertainty is preserved in the output, not resolved by coin flip.
- Whiteboard content is transcribed as-drawn, including its errors — corrections are proposed separately, clearly marked as proposals, because the board may record intent the corrector lacks context for.
- If a photo captures a credential (password on a whiteboard, sticky note on a monitor, default-password sticker), do **not** transcribe the secret — record "credential visible in photo for <system>" and flag for rotation per documentation/credential-storage-audit; the photo itself should also be treated as sensitive.
- Photos may capture bystanders' screens or papers — transcribe only the subject of the documentation request, and note if the photo should be cropped before storage.
- Degradation: without IT Glue/Hudu the cross-check step is skipped — say so; the transcription itself stands alone.
