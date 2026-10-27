---
name: public-web-research
description: Worker-only guidance for latest public web signals and domain-aware discovery. The orchestrator uses this only to create bounded IO Work Item prompts; it must not run live research itself.
---
<!-- bic:source=blank -->
# Public Web Research Worker Guidance

This is **worker guidance**, not an orchestrator action. The orchestrator must not offer to "run a live public web research sweep" for reports. It should create IO Work Item records and let the async workflow/worker execute research for reporting, auditing and reliability.

Use this guidance only when:
- drafting a bounded Search Prompt for an IO Work Item; or
- an async workflow/worker is processing an IO Work Item with Task Type = Research or Discovery.

## Critical tool requirement

This skill requires a Search all websites knowledge/web grounding, or a configured live web-search tool such as Bing Web Search, Bing Custom Search, an approved MCP web-search server, or another approved public web-search connector.

If Search all websites or another live web-search tool is not available to the worker, the worker must set the IO Work Item Search Status to Needs Remediation or Failed with a clear Worker Notes explanation. The orchestrator must not compensate by doing the research itself.

## Procedure

1. Read the bounded IO Work Item, parent IO Run, domain context profile, entity/source/template registry references and Search Prompt.
2. Do not ask follow-up questions during scheduled processing. Use only configured context and write gaps to Worker Notes.
3. Convert the context into 3-6 targeted search queries.
4. Use Search all websites or the configured web-search tool to search public sources.
5. Prefer primary sources, credible news, regulator/analyst publications, and official company pages.
6. For each candidate signal, capture title, URL, source type, published date, retrieved date, evidence excerpt, credibility tier, entity/candidate entity, theme, and implication.
7. Write IO Evidence Source and IO Finding records according to the work item contract. For scheduled work item processing, do not wait for mid-run user confirmation; use Search Status and approval items to route review.
8. For discovery, distinguish clearly relevant, potentially relevant, adjacent, and irrelevant/noise candidates; let the domain type decide what a candidate is (competitor, outlet, regulator, segment, stakeholder, risk theme, etc.).
9. Never add a new entity or source directly. Create AddEntity/AddSource approval items instead.

## Scheduled pre-run discovery mode

When invoked by a queued IO Work Item with Task Type = Discovery:

- Do not ask follow-up questions. Scheduled runs must continue without user interaction.
- Use the confirmed domain context profile, current IO Entity Registry and Source Registry.
- Use Search all websites to identify possible new or adjacent candidate entities of the domain-appropriate type (competitors, media outlets, regulators/policy topics, market segments, stakeholders, risk themes, etc.) not already registered. Let the domain type decide what to look for.
- Write candidate results as IO Finding rows with Finding Type = Candidate Entity.
- Create IO Approval Item rows with Approval Type = AddEntity and Status = Pending.
- Include proposed Entity ID, Entity Name, Entity Type, draft entity profile summary and evidence URLs in Requested Change.
- Do not create or activate IO Entity Registry rows directly.
- Do not block the main report; approved candidates join future runs.

## Orchestrator boundary

The orchestrator may:
- create the Discovery work item;
- create normal entity/source research work items;
- monitor Search Status;
- synthesize only after worker findings/evidence exist.

The orchestrator must not:
- run Search all websites itself for report findings;
- offer to run a live research sweep;
- write IO Evidence Source or IO Finding rows based on its own interactive research;
- expand the report scope with unapproved entities mid-run.

## Quality bar

- Never cite search snippets alone when a source page can be opened or retrieved.
- Do not include undated or low-credibility claims as high-confidence findings.
- Do not treat promotional vendor claims as independent evidence.
- Always include source links and retrieved dates.
- Mark uncertain candidates as candidates, not confirmed entities.
