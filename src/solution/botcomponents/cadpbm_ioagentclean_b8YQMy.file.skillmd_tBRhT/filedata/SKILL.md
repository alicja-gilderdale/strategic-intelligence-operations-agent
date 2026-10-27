---
name: initialise-domain
description: Create a new intelligence domain from a domain pattern (competitor, market/customer, regulatory/policy, account/stakeholder, PMO governance, or risk/incident), with starter registries and governance files.
---
<!-- bic:source=blank -->
# Initialise Domain

Use to add a new domain registry entry and its starter second-brain scaffolding. This does not gather intelligence itself — it makes a domain ready so entities, sources, templates and runs can be added to it. Create the IO Domain Registry row with its required Configuration ID lookup set to the selected IO Configuration.

Before creating the domain, coach the user through a lightweight domain context intake and draft a domain context profile. The domain should not be activated until the user confirms the context profile.

## When to use this skill

- The user wants to start using a domain that is not yet Active in `domains/registry.md` (for example turning on `market-customer-intelligence` after workspace setup).
- The user wants a brand-new domain not in the six starter patterns — treat this as a Generic domain and ask more clarifying questions about entity/source/template types.
- The user gives a high-level intent such as "MSFT AI competitors", "BT competitor analysis", "regulatory monitoring for X", or "PMO reporting for Y" and needs help turning it into a usable domain profile.


## Configuration vs domain decision

Before initialising a domain, confirm whether the user actually needs a new configuration.

- Use a new configuration when SharePoint root, approvers, sensitivity ceiling, business owner/team, customer workspace, or security boundary changes.
- Use a new domain when the same workspace needs a different intelligence/reporting objective.

For example, `product-features-research` for an internal product team tracking its own releases with a competitor comparison overlay should usually be a new domain in the same configuration, not a new configuration, if it uses the same SharePoint root, approver list and sensitivity ceiling.

Do not automatically classify a domain as `Competitor` just because competitor comparison is part of the output. Choose the domain type based on the primary objective:

- external competitor tracking -> Competitor;
- internal product/release tracking -> MarketCustomer or Generic;
- policy/regulatory impact -> RegulatoryPolicy.

## Supported domain patterns (from IO Use Case Patterns knowledge)

| Domain ID | Domain Type | Example output | Entity type focus | Source type focus | Template type |
|---|---|---|---|---|---|
| competitor-intelligence | Competitor | Weekly competitor brief | Competitor | Public web, analyst notes | Intelligence brief |
| market-customer-intelligence | MarketCustomer | Market signal digest | Segment, customer group | News, reports, CRM notes | Signal digest |
| regulatory-policy-monitoring | RegulatoryPolicy | Regulatory change brief | Regulator, policy topic | Regulator sites, legal updates | Impact brief |
| account-stakeholder-intelligence | AccountStakeholder | Account briefing | Account, stakeholder | CRM, meetings, public web | Account brief |
| pmo-governance-drafting | PMOGovernance | Status report, PID, RAID | Project, programme | Teams notes, documents | Governance artefact |
| risk-incident-briefing | RiskIncident | Incident briefing | Incident, risk theme | Logs, service reports, comms | Situation brief |

## Required inputs (ask if missing)

- Domain ID (stable slug, e.g. `competitor-intelligence`) and Domain Name.
- Domain Type (one of the six patterns above, or Generic).
- Owner Email (business owner accountable for domain scope/quality).
- Second Brain Path for domain-specific files.
- Domain Context Profile (drafted through the coaching questions below and confirmed by the user).

## Lightweight context coaching

Do not ask the user for a long blank-form brief. Ask 3-5 questions at a time, then draft the profile.

Start with:

1. **Viewpoint:** "Whose perspective should the intelligence be written from? For example: BT, Microsoft AI, a product team, an account team, a PMO."
2. **Objective:** "What decisions or conversations should this intelligence support?"
3. **Focus themes:** "What should the agent pay special attention to? I can suggest themes for this domain type."
4. **Scope:** "Which geography, market, product area, time horizon, or business unit matters?"
5. **Output expectation:** "What should a useful report or brief answer for you?"

Then ask only the missing follow-ups:

- priority entities or entity types to track;
- preferred sources or sources to avoid;
- exclusions/noise filters;
- cadence and reporting period;
- reviewer/approver.

Offer examples so it feels lightweight. For competitor intelligence, suggest themes such as product launches, pricing/packaging, partnerships, M&A, customer wins/losses, geography expansion, regulatory moves, AI/platform strategy, security/compliance, and implications for the viewpoint organisation.

## Domain context profile format

Draft this profile and ask the user to confirm before writing:

```md
# <Domain Name> context profile

## Viewpoint
<Organisation/product/team perspective the intelligence is written from>

## Intelligence objective
<What decisions, conversations or actions this domain should support>

## Audience and reviewers
<Primary readers, owner, approvers>

## Scope
- Geography:
- Market/product area:
- Time horizon:
- Cadence:

## Focus themes
- <Theme 1>
- <Theme 2>

## Priority entities
- <Entity or entity type>

## Source guidance
Preferred sources:
- <Source type/source>

Avoid or treat cautiously:
- <Source type/source>

## Exclusions and noise filters
- <What not to track>

## Output expectations
<What a useful brief/report must answer>

## Open questions
- <Anything the user still needs to decide>
```

## Procedure

1. Confirm the workspace is bootstrapped: active IO Configuration Default row exists and second-brain root is valid. If not, stop and use configure-io-workspace first.
2. Identify the domain type. If the user has not chosen one, present the six supported patterns and ask them to choose.
3. Run the lightweight context coaching questions and draft the Domain Context Profile.
4. Ask the user to confirm or edit the drafted profile. Do not write the domain registry row until the user confirms the profile.
5. Check `domains/registry.md` (via SharePoint MCP) for an existing row with this Domain ID.
   - If it exists and is Draft/Paused, confirm with the user whether to activate it rather than duplicating it.
   - If it exists and is Active, tell the user it is already active.
6. Confirm the proposed Domain ID, Domain Name, Domain Type, Owner Email, Second Brain Path and profile path with the user before writing anything — this is a persistent registry change.
7. Add or update the domain row in Dataverse IO Domain Registry and `domains/registry.md` with Status = Active (or Draft if the user wants to stage it first) once confirmed.
8. Create `domains/<domain-id>/profile.md` or `<Second Brain Path>/profile.md` containing the confirmed Domain Context Profile.
9. Create starter folders/files for the domain if missing, following the same starter pattern as configure-io-workspace, scoped under the domain's Second Brain Path: an entities profile folder, a sources profile folder, and a note in `templates/registry.md` pointing at the matching template(s) for this Domain Type (see the account-brief / competitor-weekly-brief / market-signal-digest / regulatory-impact-brief / pmo-status-report / incident-situation-brief templates already defined in the starter template registry).
10. Recommend next steps: add entities (maintain-entity-registry skill) and sources (maintain-source-registry skill) for the new domain before queuing a run.

## Guardrails

- Require explicit user confirmation before adding or activating a persistent domain registry entry — this is not a silent action.
- Require explicit user confirmation of the Domain Context Profile before writing it or activating the domain.
- Do not hardcode a domain to only one of the six patterns' example use cases; the same domain can support multiple entity/source/template combinations over time.
- Do not invent business context. You may propose draft themes and assumptions, but label them for user confirmation.
- Never write directly to SharePoint from this reasoning without going through the confirmed write step above.

## Web enrichment guidance

When Search all websites is available, use it to improve accuracy and completeness before creating or updating persistent setup/profile records:

- during bootstrap: validate public organisation/product naming only when relevant, but do not infer configuration values such as SharePoint path, approvers, or sensitivity ceiling;
- during domain setup: suggest focus themes, scope, aliases, known market categories, recent public context and likely source types for the draft Domain Context Profile;
- during entity setup: validate latest public details, aliases, descriptions, market role, relevant themes and candidate source URLs before drafting an entity profile;
- during source setup: validate whether a proposed source URL is official/credible and suggest source usage caveats.

All web-derived content is provisional. Show it as suggestions, cite source URLs where available, and ask the user to confirm before writing Dataverse rows or SharePoint profile files.
