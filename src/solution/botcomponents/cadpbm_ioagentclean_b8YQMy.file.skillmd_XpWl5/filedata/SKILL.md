---
name: configure-io-workspace
description: Set up or validate the IO Configuration row and SharePoint second-brain location, and scaffold the starter second-brain folder structure.
---
<!-- bic:source=blank -->
# Configure IO Workspace

Use when the user starts setup, asks where the solution is configured, or when the startup routine finds no active or complete IO Configuration row.

This is the mandatory **first-run bootstrap skill**. The agent must run this before any intelligence workflow if the workspace has not been configured.

## When to use this skill

- First-time setup of the Intelligence Operations workspace.
- The user greets the agent or asks "where do I start" and the Default configuration row is missing or incomplete.
- The user asks for a run, report, draft, domain setup, registry update, synthesis or publication before the Default configuration row is valid.
- The user asks "where is this configured" / "what SharePoint site does this use" / wants to change the approver list or sensitivity ceiling.
- The startup routine could not read an active IO Configuration row or could not validate the configured second-brain root.

## Required inputs (ask if missing — never infer)

- Solution Name (display name for this deployment).
- SharePoint Site URL and Folder Path for the second brain root.
- Sensitivity Ceiling (Public, General, Confidential or HighlyConfidential).
- Approver Email List (semicolon separated).

Do not infer customer, tenant, domain or any of the above from the environment name, browser URL, or the requesting user's email domain — always ask and let the user confirm.

## Procedure

1. Read IO Configuration where Configuration ID = Default.
2. If the row does not exist, say: "This looks like a first-time setup. I need to bootstrap the Intelligence Operations workspace before I can run intelligence tasks."
3. If the row exists but has missing required values, say which values are missing and stay in setup mode.
4. Ask the user for Solution Name, SharePoint Site URL, Folder Path, Sensitivity Ceiling and Approver Email List (skip any the user has already provided).
5. Check whether an IO Configuration row with Configuration ID = Default already exists.
   - If it exists, show the current values and ask whether the user wants to update any of them.
   - If it does not exist, confirm the collected values back to the user before creating it.
6. Create or update the IO Configuration Default row with Is Active = Yes through Dataverse MCP.
7. Validate that the SharePoint folder exists at the configured Site URL + Folder Path through SharePoint MCP.
8. Check for the starter second-brain structure under the configured folder; if any part is missing, offer to create it:
   - `domains/registry.md`
   - `entities/registry.md` and `entities/profiles/`
   - `sources/registry.md` and `sources/profiles/`
   - `templates/registry.md`, `templates/profiles/`, `templates/template_files/`
   - `docs/reports`, `docs/learned`, `docs/decisions`, `docs/architecture`, `docs/governance`
9. Never overwrite an existing file without explicit confirmation from the user for that specific file.
10. Seed governance starter content only when creating `docs/governance` for the first time, for example a `sensitivity-and-approvals.md` capturing: default sensitivity ceiling, mandatory human review before publishing/sharing, refusal of above-ceiling source content, approval required for registry changes that add sources/entities/templates, and the rule that generated outputs must contain citations or review markers.
11. Report setup status back to the user (what was created, what was updated, what was left unchanged) and recommend the next step — usually the initialise-domain skill for the user's first intelligence domain.

## Guardrails

- Never infer customer or domain from tenant, environment, URL or email domain.
- Only one Default active IO Configuration row should exist; if another Is Active = Yes row is found, flag it to the user instead of silently changing it.
- This skill only creates/validates structure and configuration; it does not add domains, entities, sources or templates — hand off to initialise-domain, maintain-entity-registry or maintain-source-registry for that.
- If setup is incomplete, do not run queue-intelligence-run, synthesise-output, publish-approved-output, or draft-from-template.

## Web enrichment guidance

When Search all websites is available, use it to improve accuracy and completeness before creating or updating persistent setup/profile records:

- during bootstrap: validate public organisation/product naming only when relevant, but do not infer configuration values such as SharePoint path, approvers, or sensitivity ceiling;
- during domain setup: suggest focus themes, scope, aliases, known market categories, recent public context and likely source types for the draft Domain Context Profile;
- during entity setup: validate latest public details, aliases, descriptions, market role, relevant themes and candidate source URLs before drafting an entity profile;
- during source setup: validate whether a proposed source URL is official/credible and suggest source usage caveats.

All web-derived content is provisional. Show it as suggestions, cite source URLs where available, and ask the user to confirm before writing Dataverse rows or SharePoint profile files.


## Multiple configurations / reporting scopes

This solution supports multiple IO Configuration rows. `Default` is only the bootstrap fallback, not the only supported configuration.

Use this skill when the user wants to set up a new reporting scope for a different customer, product, market, geography, domain, or report context.

For a new configuration, collect and confirm:

- Configuration ID (stable unique key, e.g. `MSFT-AI-Competitors` or `BT-Competitor-Weekly`)
- Solution Name
- SharePoint Site URL
- Folder Path
- Sensitivity Ceiling
- Approver Email List
- Optional Default Domain
- Is Active

Before writing, show whether this will create a new IO Configuration row or update an existing one. Do not overwrite or deactivate other configurations unless the user explicitly asks.

After creating a new configuration, recommend creating the first domain context profile under that reporting scope.
