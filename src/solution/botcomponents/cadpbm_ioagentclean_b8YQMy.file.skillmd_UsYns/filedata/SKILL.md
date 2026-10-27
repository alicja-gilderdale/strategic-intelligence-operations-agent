---
name: queue-intelligence-run
description: Create IO Run and IO Work Items for a recurring or ad hoc request, decomposing work using domain/entity/source/template registries.
---
<!-- bic:source=blank -->
# Queue Intelligence Run

Use domain/entity/source/template registries to create bounded work items for a run. Avoid hardcoded task counts; create one work item per entity/source/query combination where useful, following the domain's work decomposition pattern.

## When to use this skill

- The user asks to run/kick off/schedule an intelligence operation, e.g. "run this week's competitor brief" or "start the PMO status report for Project X".

## Required inputs (ask if missing)

- Domain ID.
- Template ID (the desired output template).
- Period Start / Period End (or an incident window for risk/incident domains).
- Optional entity filter and/or source filter (default to all Active entities/sources for the domain if the user does not want to filter).
- Requested By (the requesting user).

## Procedure

1. Read the IO Configuration Default row and confirm Is Active = Yes.
2. Read the IO Domain Registry row for the Domain ID and confirm Status = Active. If not active, stop and point the user at initialise-domain.
3. Read the IO Template Registry row for the Template ID and confirm Agent Ready = Yes. If not ready, stop and explain what is missing (do not draft with a non-ready template).
4. Resolve the Active entities and Active sources for the domain from the entity/source registries, applying any filter the user gave.
5. If there are zero active entities, zero active sources, or the template is not ready, do **not** create an empty run — report this setup gap back to the user and suggest the relevant fix (maintain-entity-registry, maintain-source-registry, or escalate the template readiness gap).
6. Create the IO Run with Search Status = Queued, the selected Configuration ID lookup, the resolved Domain ID, Run Type (Scheduled/AdHoc/Remediation/TemplateDraft as appropriate), Requested By, Period Start/End and Output Template ID. Set the visible Run ID using the exact pattern domain-id-yyyyMMdd-HHmmss, for example product-features-research-20260730-152336. Do not include week numbers, run type labels, template IDs or run- prefixes in the Run ID; those belong in structured IO Run fields.
7. For Competitor domains, create one non-blocking pre-run discovery work item before the normal entity research work items:
   - Task Type = Competitor Discovery.
   - Search Status = Queued.
   - Entity ID and Source ID may be blank.
   - Search Prompt: "Use Search all websites and the domain context profile to identify possible new or adjacent competitors not already in IO Entity Registry. Write candidate findings and AddEntity approval items only. Do not add entities directly. Do not block the main run."
8. Create IO Work Item records using the domain's work decomposition pattern:
   - Competitor / market-customer / regulatory / account domains: one work item per entity/source/query cluster.
   - PMO governance drafting: one extract/draft work item per source document or transcript.
   - Risk/incident briefing: one extract/summarise work item per source stream or incident window.
9. Set every new IO Work Item Search Status = Queued and Retries = 0, with a bounded Search Prompt describing exactly what that work item should investigate.
10. Report the Run ID and work item count back to the user, and note that processing happens automatically via the process-intelligence-work-item flow (see `_pending_tools/automated-workflow-and-inline-worker.md` once that flow is configured) — the agent's process-work-item-result skill is used to check on and remediate results afterwards.

## Scheduled-run rule

Scheduled runs must not stop in the middle to ask the user follow-up questions. Use the confirmed domain context profile, active registries and configured templates. If pre-run discovery finds possible new competitors, create Candidate Entity findings and AddEntity approval items, then continue the weekly report using only currently approved Active entities.

Candidate competitors from pre-run discovery join future runs only after approval, unless the user explicitly asks to re-queue the current run.

## Guardrails

- Never hardcode task counts; the number of work items should follow from the number of active entities/sources/documents relevant to the request.
- Never create a run against a Draft/Paused/Retired domain, a non-Agent-Ready template, or with zero resolvable entities/sources.
- Do not invent entities or sources that are not in the registry; if the user names something not yet tracked, offer to add it via maintain-entity-registry / maintain-source-registry first.
- The orchestrator creates discovery work items and approval items; it does not execute per-entity research itself.
