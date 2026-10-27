# Work item lifecycle, retries and review policy

## Statuses

| Status | Meaning |
|---|---|
| Draft | Created but not yet released to a worker. Draft items never trigger processing. |
| Queued | Released for processing. |
| In Progress | A worker is running. |
| Completed | Evidence and findings are present and well formed. |
| No Material Activity | The worker found nothing material for this entity and source in the period. This is a legitimate result, not an error. It means nothing was verified, not that nothing happened. |
| Needs Remediation | Research was incomplete but can be retried. Used on attempts 1 and 2 only. |
| Needs Review | A human decision is needed on usable findings. Used only after the final allowed attempt. |
| Failed | Setup or system defect, or the final attempt produced no usable findings. |
| Skipped | Deliberately not processed. |

## Retry rules

- A work item gets up to **three attempts**. `Retries` counts attempts.
- The worker workflow picks up items that match:

```text
cadpbm_status eq 267730001 or (cadpbm_status eq 267730006 and cadpbm_retries lt 3)
```

  That is `Queued`, or `Needs Remediation` with fewer than three retries.
- **Attempts 1 and 2:** incomplete but retryable research (transient failure, incomplete search, source gap) ends as **Needs Remediation** and is retried automatically.
- **After the final attempt:** if at least one usable finding still needs a human decision, the item ends as **Needs Review**. If nothing usable was found, it ends as **Failed**.
- A defect in setup or the system (for example a missing entity or source on the work item) is tagged `[SETUP]` in Worker Notes and ends as **Failed**. It never reaches a reviewer.
- The real error is always kept in **Worker Notes**. It is never replaced with a generic message.
- An item is **Completed**, not Needs Review, when it has usable findings and every one is Ready For Synthesis, even if the worker flagged it.

Needs Review is reserved for usable ambiguity. Incomplete research is retried and broken setup is failed, so reviewers see only decisions that need judgement.

## Run readiness

A run is ready for synthesis when every non-discovery work item is in a terminal status: Completed, No Material Activity, Needs Review, Failed or Skipped. The discovery item does not block synthesis.

## Queueing safeguards

The `queue-intelligence-run` skill avoids queueing work that cannot succeed:

1. **Pre-flight:** each non-discovery item must have an Entity ID (and Source ID where the task is source-scoped), a non-empty bounded Search Prompt and a Task Type.
2. **Create as Draft:** all items are created as Draft with `Retries = 0`, with every required field set in the same create.
3. **Validate the batch:** re-read every item and check that entities and sources belong to the run's domain and are Active, there are no duplicates and the count matches the plan.
4. **Queue last:** only when the batch passes, update every valid item to Queued. Queueing starts the workers, so it is always the final write.

## Review load policy

Findings are classified so reviewers focus on what needs a decision.

| Finding | Status | Review |
|---|---|---|
| High confidence | Ready For Synthesis | Auto-accepted |
| Medium confidence with credible independent support | Ready For Synthesis | Auto-accepted, spot-check 10-20% |
| Low confidence | Needs Review | Must review |
| High-impact Risk below High confidence | Needs Review | Must review |
| Single vendor or self-reported claim | Needs Review | Must review |
| Candidate Entity | Needs Review | Must review, and creates an AddEntity approval |

Interpretation, Forward-looking and Not independently validated are labels on a finding, not review triggers.

Reports state the effort up front, for example `9 must-review, 39 spot-check, 72 auto-accepted`, and list the must-review findings first with a `[REVIEW: reason]`. Finding summaries are 500 characters or fewer; the detail lives in the evidence sources.

## Report output

Synthesis saves each report in the domain's `reports` folder as `<run-id>.html` and `<run-id>.md`. The IO Output Artifact's **SharePoint URL** always points to the **HTML** file, which is the report reviewers open. The Markdown link is kept in the Citation Map as a secondary link.
