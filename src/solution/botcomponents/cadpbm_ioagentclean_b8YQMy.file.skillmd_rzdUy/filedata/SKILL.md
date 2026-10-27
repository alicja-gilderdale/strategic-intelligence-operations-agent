---
name: maintain-source-registry
description: Add, update or retire approved intelligence sources with credibility tier and allowed use, across public web, SharePoint, M365, CRM, Dataverse, manual upload and Blob source types.
---
<!-- bic:source=blank -->
# Maintain Source Registry

Use for public web, SharePoint, M365, CRM, Dataverse, manual upload and Blob source profiles (`IO Source Registry` in Dataverse, mirrored at `sources/registry.md`).

## When to use this skill

- The user wants to approve a new source for a domain, change a source's allowed use or credibility tier, or retire/pause a source.

## Required inputs (ask if missing)

- Domain ID the source belongs to (must be Active).
- Source Name, Source Type (PublicWeb, SharePoint, M365Meeting, M365Email, TeamsChat, CRM, Dataverse, ManualUpload or AzureBlob) and Source URL/location where applicable.
- Allowed Use (ReadOnly, ReadWriteDrafts, ApprovalRequired or Disabled).
- Credibility Tier (Primary, TrustedSecondary, AnalystProvided or Unverified).
- A stable Source ID slug (derive from the source name if not supplied, and confirm it).

## Procedure

1. Confirm the target domain is Active; if not, point the user at initialise-domain.
2. For a new source: propose Source ID, Source Name, Source Type, Source URL, Allowed Use, Credibility Tier and Status = Active; confirm with the user, especially Allowed Use since it controls what side effects the source permits.
3. For an update/retirement: show current values and confirm exactly what changes, particularly any change that increases Allowed Use privileges (e.g. ReadOnly → ReadWriteDrafts) — always treat privilege increases as requiring approval.
4. Hand off to the request-approval skill (Approval Type = AddSource, or an equivalent change) before writing any new source or any change that increases its allowed use or credibility tier, unless the domain's governance explicitly allows unapproved changes.
5. Once approved (or not required), create/update the IO Source Registry row, and update `sources/registry.md` and the source's profile file (`sources/profiles/<source-id>.md`) describing intended use, e.g. "use for public, credible sources only; prefer primary sources when available" style guidance.
6. Confirm back to the user what was written.

## Guardrails

- Never assign Credibility Tier = Primary or Allowed Use above ReadOnly without the user explicitly confirming that assignment.
- Only Active sources are used automatically by queue-intelligence-run; Needs Review/Paused/Retired sources are excluded.
- Do not register a source with an Allowed Use of ReadWriteDrafts or beyond unless the requesting user has confirmed that write scope is intended.

## Web enrichment guidance

When Search all websites is available, use it to improve accuracy and completeness before creating or updating persistent setup/profile records:

- during bootstrap: validate public organisation/product naming only when relevant, but do not infer configuration values such as SharePoint path, approvers, or sensitivity ceiling;
- during domain setup: suggest focus themes, scope, aliases, known market categories, recent public context and likely source types for the draft Domain Context Profile;
- during entity setup: validate latest public details, aliases, descriptions, market role, relevant themes and candidate source URLs before drafting an entity profile;
- during source setup: validate whether a proposed source URL is official/credible and suggest source usage caveats.

All web-derived content is provisional. Show it as suggestions, cite source URLs where available, and ask the user to confirm before writing Dataverse rows or SharePoint profile files.
