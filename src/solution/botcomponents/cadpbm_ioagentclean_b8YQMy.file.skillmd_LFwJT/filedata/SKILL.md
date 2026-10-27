---
name: learn-from-run
description: Propose learned-context updates from a completed run — durable lessons, source gaps, template improvements and registry improvements. Creates an approval item before updating docs/learned or registries.
---
<!-- bic:source=blank -->
# Learn From Run

Use after a run has been published (or reviewed) to capture durable lessons for next time. This skill only proposes changes; it never writes them directly.

## When to use this skill

- After publish-approved-output completes, or when the user asks "what did we learn from this run" / "should we adjust our sources/templates".

## Procedure

1. Review the run's IO Findings, IO Evidence Sources, Worker Notes and the published Output Artifact.
2. Summarise durable lessons: recurring source gaps (sources that consistently produced No Material Activity or low-credibility evidence), template improvements (sections that were hard to fill or often needed [REVIEW] markers), and registry improvements (entities/sources that should change Status or Priority).
3. Draft the proposed update to `docs/learned/context.md` (or the relevant registry) — keep it factual and free of unapproved or sensitive detail.
4. Hand off to request-approval with Approval Type = UpdateSecondBrain (or AddEntity/AddSource if the lesson implies a registry change) before writing anything.
5. Only after Approved, write the update to `docs/learned/context.md` and/or the relevant registry entry.

## Guardrails

- Never write to `docs/learned` or any registry without an Approved approval item first — this file only stores approved durable lessons.
- Do not include private, sensitive or unapproved details in learned-context updates.
- Keep proposed lessons specific and evidence-based; do not speculate beyond what the run's findings support.
