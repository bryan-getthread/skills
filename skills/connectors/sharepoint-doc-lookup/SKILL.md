---
name: SharePoint Doc Lookup
description: Answer a ticket question from the client's SharePoint document libraries via the member's connected M365 — search files and folders, cite the links, and degrade to the KB / IT Glue / Hudu when SharePoint isn't connected. Answers the commonly requested "the answer is in our SharePoint" workflow; runs manually on demand.
category: Connectors
tools: [sharepoint_search, sharepoint_folder_search, search_knowledge_base, search_itglue, search_hudu]
---

# SharePoint Doc Lookup

A lot of client-specific answers (procedures, contacts, standards, forms) live in a SharePoint library, not the KB. When a ticket needs one, search the member's connected SharePoint, pull the answer, and cite the source link — falling back to Thread's own documentation when SharePoint isn't available.

## When to use

- "What does <client>'s SharePoint say about <procedure>?"
- "Find the <document> in the client's SharePoint for this ticket"
- "Is there a runbook / form / policy in SharePoint for this?"
- Answering a ticket where the authoritative doc is known to live in M365, not the KB

## Steps

1. **Connection check first.** This depends on the member having connected their Microsoft 365 (SharePoint) via the M365 connector. If it isn't connected, say so plainly and skip straight to the degrade path (step 5) — don't pretend to search.
2. Frame the query from the ticket: the client/site, the topic, and the document type if known. Use sharepoint_search for a content/keyword search across libraries, and sharepoint_folder_search when the member points at (or you can identify) a specific library or folder to narrow scope.
3. Read the top matches and extract the specific answer the ticket needs — the relevant passage, value, or step — not just a list of file names.
4. Answer on the ticket in plain language, and **cite the SharePoint link(s)** for each fact so the tech can open the source. If several documents conflict or one looks stale (old modified date), flag that rather than picking silently.
5. **Degrade path.** If SharePoint is not connected, returns nothing, or the member lacks access: fall back to search_knowledge_base, then search_itglue / search_hudu if enabled. State which source the answer came from, and if none has it, say the doc wasn't found in any connected source rather than guessing.

## Guardrails

- Member-authenticated: only searches SharePoint the connected member already has access to. Never attempt to reach content outside their permissions, and note in output that results are scoped to their access.
- Cite real links only — every SharePoint or KB reference must be an actual result. Never fabricate a document title, path, or URL.
- Answer from the document, don't paraphrase into inaccuracy; when quoting a procedure, preserve the exact steps/values.
- Flag stale documents (old last-modified) instead of presenting them as current.
- Do not copy sensitive content (credentials, PII) into the ticket body — link to the source and summarize instead.
- Member-invoked only; there is no unattended variant (Flows can't schedule this and can't hold the member's M365 auth).
