---
name: Flow Backup Export
description: Dump every flow definition to a JSON or markdown snapshot for archive and diffing — a point-in-time backup of the desk's automation, read-only with no restore path. Answers the commonly requested "back up / snapshot our flows" workflow; runs manually on demand.
category: Automation & Flows
tools: [list_flows, get_flow]
---

# Flow Backup Export

Automation drifts, and there's no undo for a flow someone edited three weeks ago. Capture a full snapshot of every flow's definition so you have something to archive, diff against later, or reference when a flow's behavior changes unexpectedly.

## When to use

- "Back up all our flows"
- "Snapshot the flow definitions so I can diff them later"
- "Export every flow to JSON / markdown for the archive"
- Before a big automation cleanup, or to establish a known-good baseline

## Steps

1. List every flow with list_flows, then get_flow for each to pull the full definition: trigger, filter conditions, actions, notification targets, and enabled/disabled state.
2. Serialize the collection into the requested format:
   - **JSON** — the faithful machine-readable dump, best for diffing two snapshots.
   - **Markdown** — a human-readable one-section-per-flow rendering, best for review and archiving alongside docs.
3. Stamp the snapshot with the capture date/time and the flow count at the top, so a later diff knows what it's comparing.
4. Deliver the file/content and state clearly: this is a **point-in-time snapshot** and re-running produces a fresh one.

## Guardrails

- **Read-only, and no restore path.** State this explicitly: this skill *exports* flow definitions but cannot re-import or restore them. It's for archive and diff, not recovery — don't imply a snapshot can be "loaded back".
- Capture faithfully — the dump must reflect exactly what get_flow returns; never summarize away fields that matter for a diff, and never fabricate a flow or field.
- Include the capture timestamp and flow count so snapshots are comparable.
- Don't include secrets — if a flow definition surfaces credential-like values, note their presence rather than dumping them in plain text into an archive.
- Member-invoked only; no unattended variant (Flows can't schedule an export).
