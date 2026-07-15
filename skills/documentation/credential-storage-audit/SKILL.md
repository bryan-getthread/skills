---
name: Credential Storage Audit
description: Map where a client's credentials actually live — vault, doc platform, tickets, spreadsheets — report findings by risk, and produce the migration plan to the vault; any plaintext credential found in a ticket triggers rotate-and-flag.
category: Documentation
tools: [search_itglue, search_hudu, search_tickets, search_knowledge_base, notion-search, add_ticket_note, create_ticket]
---

# Credential Storage Audit

Finds the credential sprawl for one client — the passwords living in ticket threads, wiki pages, and "passwords.xlsx" instead of the vault — and turns it into a risk-ranked findings report plus a migration plan. Plaintext credentials in tickets are treated as exposed: rotate and flag, not just relocate.

## When to use

- "Audit where <client>'s credentials are actually stored."
- Onboarding an acquired client book, or offboarding a technician, and the desk needs to know what's exposed where.
- A password was just found sitting in a ticket thread and the team suspects it's not the only one.
- Pre-audit / cyber-insurance hygiene: "prove our credentials live in the vault."

## Steps

1. Scope with the requester: one <client> (recommended — run per client to stay within search caps), and which vault is the sanctioned destination (IT Glue passwords, Hudu passwords, or a dedicated password manager outside this tool surface).
2. Sweep the storage locations, one search pass per surface:
   - **Doc platforms** (search_itglue / search_hudu): credential-type entries (in the right place — inventory them) AND free-text documents whose titles/content suggest embedded secrets ("password", "credentials", "login", "admin account", locale equivalents).
   - **Tickets** (search_tickets): threads and notes containing password-handoff patterns ("the password is", "temp password", "login:", "pw:"). Split searches per signal — result caps are real; report each search as capped/uncapped.
   - **KB** (search_knowledge_base) and **Notion** (notion-search, if the member has connected it): setup guides and runbooks with hardcoded secrets.
   - **Spreadsheets** can only be found by reference — flag any doc/ticket that *mentions* a password spreadsheet ("see the password sheet on the share") as a finding of its own for a human to chase.
3. Classify every finding by risk, highest first:
   - **Critical — plaintext in tickets:** visible to anyone with ticket access, often synced into the PSA, effectively exposed. → rotate-and-flag (step 4).
   - **High — plaintext in free-text docs/KB/Notion pages:** readable by everyone with doc access, outside vault access controls and audit logging.
   - **High — referenced external spreadsheets:** unmanaged, copyable, unaccounted for.
   - **Medium — vault entries that are stale or over-shared** (where metadata shows it).
   - **Info — properly vaulted:** the baseline inventory.
4. **Rotate-and-flag rule (non-negotiable):** every plaintext credential found in a ticket is treated as compromised. For each: create_ticket (or add_ticket_note on an existing remediation ticket) for a human to rotate that credential, referencing the finding by ticket number and system name — **never by quoting the credential**. Migration without rotation is not an acceptable close for this class.
5. Produce the **findings report**: per finding — location (ticket number / doc title), system it unlocks, risk class, recommended action (rotate + vault / migrate to vault / verify + delete source). Then the **migration plan**: ordered by risk class, each item = move to vault → verify it works from the vault copy → delete the source copy → note the cleanup. State explicitly which searches hit result caps and are therefore partial.

## Guardrails

- **Never reproduce a credential** in the report, a note, a new ticket, or chat — reference every secret by system/account label only. The audit must not become another exposure.
- Never edit or delete anything: no source cleanup, no vault writes — the output is findings + plan; humans execute migration and deletion.
- Rotate-and-flag for ticket-found credentials is mandatory, not advisory — recommend rotation even if the team believes exposure was brief.
- Result-cap honesty: keyword sweeps miss creatively-stored secrets and capped searches miss volume; state the audit is a lower bound, not a guarantee of completeness.
- Notes/tickets created are plain text per the Note Format Standard skill (documentation/note-format-standard).
- Degradation: without IT Glue/Hudu the doc-platform pass is skipped — say so; Notion requires the member's connection and only sees that member's access. A vault outside the tool surface is inventoried from the requester's answers only.
