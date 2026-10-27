# Solution overview

Strategic Intelligence Operations is a reusable agentic pattern for recurring and ad hoc intelligence work. It supports competitor briefs, market and customer signals, regulatory monitoring, account briefings, PMO drafting, and risk or incident reporting.

## Problem

Intelligence workflows often rely on long prompts, manual research, copied evidence, and untracked updates. This makes outputs difficult to audit, repeat, govern, and improve.

## Approach

The solution introduces:

- Dataverse records for configuration, domains, entities, sources, templates, runs, work items, evidence, findings, outputs, and approvals;
- SharePoint files for human-readable profiles, templates, reports, learned context, and decisions;
- a Copilot Studio orchestrator that queues bounded work rather than performing every task in one conversation;
- an asynchronous worker workflow that writes auditable evidence and findings;
- human review before publishing or changing persistent knowledge.

## Reuse model

An **IO Configuration** defines the workspace boundary: SharePoint location, approvers, sensitivity ceiling, and solution identity.

An **IO Domain** defines an intelligence job inside that workspace: objective, audience, tracked entities, sources, cadence, and output template.

Create a new domain when the intelligence topic changes. Create a new configuration when the workspace, approvers, or sensitivity boundary changes.
