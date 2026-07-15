---
name: Screenshot OCR Translate
description: Extract the text from a pasted screenshot — an error dialog, email, or app screen — translate it to English if needed, and summarize what it actually says.
category: Communication
tools: [add_ticket_note]
---

# Screenshot OCR Translate

Turns a screenshot into working text: exact transcription, English translation when the content is in another language, and a one-line read of what it means.

## When to use

- A client or tech pastes a screenshot of an error message, dialog, or email — "what does this say?"
- A foreign-language screenshot needs translating before anyone can troubleshoot it.
- A ticket attachment is an image of text that should be searchable/quotable in the record.

## Steps

1. Extract the text from the image, structured to mirror the screenshot: window/dialog titles, body text, button labels, and any codes or paths — each transcribed **exactly**, including error codes, hex values, filenames, and URLs character-for-character.
2. If the extracted text is not in English, provide the English translation below the transcription, section by section, keeping error codes and technical identifiers untranslated.
3. Summarize in 1–3 sentences: what kind of message this is (error, warning, phishing email, license prompt), what it's saying, and — only when the text itself makes it unambiguous — what it implies (e.g., "a disk-full warning from Windows, saying drive C: has less than 200 MB free").
4. Mark anything unreadable honestly: [illegible] for text you can't make out, [cut off] for truncated content. Never reconstruct blurred text into a guess presented as transcription.
5. If working a ticket, offer to store the transcription (and translation) as a plain-text internal note via add_ticket_note so the image's content becomes searchable in the record.

## Guardrails

- Transcription is verbatim or flagged — an OCR'd error code with one wrong character sends troubleshooting down the wrong path. When uncertain about a character, show it as [?] rather than choosing.
- Keep the summary descriptive, not diagnostic: say what the message says; don't leap to a root cause or a fix (route to troubleshooting workflows for that).
- If the screenshot contains credentials, one-time codes, or personal data, transcribe around them — note "[password visible in screenshot — not transcribed]" and warn the tech the image itself contains sensitive data.
- If the screenshot looks like a phishing email, say so as a possibility and stop at description — phishing verdicts belong to the security workflow.
- Notes in plain text only (PSA compatibility).

## Unattended (Flows) variant

- Your entire reply is the note content: transcription, translation (if any), one-line summary — no narration.
- If the image contains no legible text, output nothing.
