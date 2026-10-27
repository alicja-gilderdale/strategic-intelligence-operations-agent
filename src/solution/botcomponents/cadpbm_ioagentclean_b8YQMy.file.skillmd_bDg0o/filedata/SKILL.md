---
name: process-work-item-result
description: Interpret worker results for IO Work Items and decide whether remediation or synthesis is needed. Never publishes.
---
<!-- bic:source=blank -->
# Process Work Item Result

Use to validate evidence and findings written by the inline worker for one or more IO Work Items, and to decide the right next status. This skill never publishes anything.

## When to use this skill

- The user asks for the status of a run/work items, or asks the agent to check whether processing has finished or needs attention.
- A work item has come back Needs Review or Needs Remediation and the user wants the agent's read on what to do.

## Background: work item lifecycle

Work items move through Search Status values: Draft → Queued → In Progress → one of Completed, No Material Activity, Needs Review, Needs Remediation, Failed, Skipped (see `choices_and_status_codes.md` knowledge for exact values). Processing itself is performed by an automated flow and inline worker (see `_pending_tools/automated-workflow-and-inline-worker.md`); this skill is how the agent reasons about the *results* of that processing, not how it performs the research itself.

## Procedure

1. Read the IO Work Item(s) for the run in question, including Search Status, Retries and Worker Notes.
2. For each work item, read its linked IO Evidence Source and IO Finding rows.
3. Validate:
   - Findings reference evidence IDs where evidence exists.
   - Findings are not vague or unsupported — flag any finding lacking evidence as Needs Review rather than accepting it.
   - No Material Activity is a legitimate terminal state, not a failure, when a work item genuinely found nothing.
4. Decide/report the appropriate work item Search Status:
   - **Completed** — evidence and findings are present and well-formed.
   - **No Material Activity** — the worker found nothing material for this entity/source in the period; this is expected, not an error.
   - **Needs Review** — findings exist but are ambiguous, borderline sensitive, or need a human's judgement before being used.
   - **Needs Remediation** — the work item should be retried (e.g. transient failure, incomplete search) — only recommend this while Retries is at or below the configured retry ceiling (3, per the workflow contract).
   - **Failed** — Retries has exceeded the ceiling and the item is still not resolved; explain this to the user and suggest a manual fix (e.g. adjust the Search Prompt, source, or entity).
5. Once all work items for a run are in a terminal state (Completed, No Material Activity, Needs Review, Failed or Skipped), tell the user the run is ready for synthesis and hand off to the synthesise-output skill (do not synthesise here).
6. If any work item is Failed, flag that the run may need Review Required handling rather than full automatic synthesis, unless the user confirms partial synthesis is acceptable.

## Guardrails

- Do not publish, approve, or write outputs from this skill — it only classifies and reports on work item Search Status.
- Do not mark a work item Completed if its findings lack any evidence reference and evidence was expected.
- Respect the Retries ceiling (3) before recommending Failed.
