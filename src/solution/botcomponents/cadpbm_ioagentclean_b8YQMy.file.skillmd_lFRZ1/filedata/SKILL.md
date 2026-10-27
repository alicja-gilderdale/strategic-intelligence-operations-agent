---
name: recover-second-brain
description: Rebuild missing second-brain starter files from Dataverse configuration. Uses Dataverse as the source of truth and asks before overwriting existing files.
---
<!-- bic:source=blank -->
# Recover Second Brain

Use when the SharePoint second brain is missing, partially deleted, or suspected out of sync with Dataverse, and the user wants it rebuilt.

## When to use this skill

- The user reports missing/broken registry files, or the startup routine cannot find expected second-brain files even though IO Configuration exists.

## Procedure

1. Read IO Configuration to confirm the SharePoint Site URL and Folder Path.
2. Check which starter files/folders are missing: `domains/registry.md`, `entities/registry.md` (+ `entities/profiles/`), `sources/registry.md` (+ `sources/profiles/`), `templates/registry.md` (+ `templates/profiles/`, `templates/template_files/`), and `docs/reports`, `docs/learned`, `docs/decisions`, `docs/architecture`, `docs/governance`.
3. For each missing registry file, rebuild its rows from the corresponding Dataverse table (IO Domain Registry, IO Entity Registry, IO Source Registry, IO Template Registry) — Dataverse is the source of truth, not any local cache or assumption.
4. For each missing profile file, rebuild a minimal profile from the Dataverse row's fields (Entity Type/Status/Priority, or Source Type/Allowed Use/Credibility Tier) and note that free-text narrative content (tracked themes, notes) could not be recovered and should be re-added by the user if known.
5. Before writing anything, ask the user to confirm the recovery plan; if any target file already exists, explicitly ask before overwriting it, and prefer to write to a side-by-side "recovered" file if the user is unsure.
6. Report what was recovered, what could not be fully recovered (free-text narrative content), and recommend the user review the recovered files.

## Guardrails

- Dataverse records are the source of truth for recovery; do not fabricate registry rows that have no Dataverse counterpart.
- Always ask before overwriting an existing file.
- Clearly flag any content that could not be recovered (e.g. narrative notes not stored in Dataverse) rather than inventing plausible-sounding text.
