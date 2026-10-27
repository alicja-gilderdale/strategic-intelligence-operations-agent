---
name: maintain-entity-registry
description: Add, update or retire tracked entities such as competitors, accounts, regulators, projects, incidents, risk themes and market segments.
---
<!-- bic:source=blank -->
# Maintain Entity Registry

Use for competitors, accounts, regulators, projects, incidents, risk themes and market segments tracked within a domain (`IO Entity Registry` in Dataverse, mirrored at `entities/registry.md` in the second brain).

## When to use this skill

- The user wants to add a new entity to track (e.g. "start tracking Competitor X"), update an existing entity's status/priority, or retire one.

## Required inputs (ask if missing)

- Domain ID the entity belongs to (must be an Active domain — check `domains/registry.md` first).
- Entity Name and Entity Type (Competitor, Customer, MarketSegment, Regulator, PolicyTopic, Account, Stakeholder, Project, Programme, RiskTheme or Incident).
- Status (Active, Watchlist, Paused or Retired) and Priority (Low, Medium, High or Critical) for a new entity — default Status to Watchlist and Priority to Medium if the user has no preference, but confirm before saving.
- A stable Entity ID slug (derive from the entity name if the user does not supply one, and confirm it).

## Procedure

1. Confirm the target domain is Active; if not, stop and point the user at initialise-domain.
2. For a new entity: check the entity is not already registered under the same Domain ID; propose the Entity ID, Entity Name, Entity Type, Status and Priority; confirm with the user.
3. For an update/retirement: read the existing entity row, show the current values, and confirm exactly what will change (especially Status changes to Paused/Retired, which affect whether work items are generated for it).
4. Determine whether this domain's governance requires approval for entity changes (check the domain's governance notes / IO Configuration policy, and default to requiring approval when in doubt). If approval is required, hand off to the request-approval skill with Approval Type = AddEntity (or a status-change equivalent) before writing the change.
5. Once approved (or if no approval is required), create or update the IO Entity Registry row, and update `entities/registry.md` and the entity's profile file (`entities/profiles/<entity-id>.md`) with the entity's tracked themes/notes.
6. Confirm back to the user what was written.

## Guardrails

- Never invent an entity's tracked themes, priority or status — ask, or leave the profile's "notes" section minimal until the user supplies more.
- Create an approval item for changes when domain governance requires approval; do not skip this because it seems like a small change.
- Only Active entities are used for run/work-item generation — Watchlist/Paused/Retired entities are excluded automatically by queue-intelligence-run.

## Web enrichment guidance

When Search all websites is available, use it to improve accuracy and completeness before creating or updating persistent setup/profile records:

- during bootstrap: validate public organisation/product naming only when relevant, but do not infer configuration values such as SharePoint path, approvers, or sensitivity ceiling;
- during domain setup: suggest focus themes, scope, aliases, known market categories, recent public context and likely source types for the draft Domain Context Profile;
- during entity setup: validate latest public details, aliases, descriptions, market role, relevant themes and candidate source URLs before drafting an entity profile;
- during source setup: validate whether a proposed source URL is official/credible and suggest source usage caveats.

All web-derived content is provisional. Show it as suggestions, cite source URLs where available, and ask the user to confirm before writing Dataverse rows or SharePoint profile files.
