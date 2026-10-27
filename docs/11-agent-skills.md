# Agent skills and models

The Copilot Studio agent uses the skills below. Skill source is under `src/solution/botcomponents`.

| Skill | Purpose |
|---|---|
| `configure-io-workspace` | Validates the active IO Configuration and SharePoint structure |
| `initialise-domain` | Creates a domain, its context profile and registry entries |
| `maintain-entity-registry` | Adds or changes entities, with approval for persistent changes |
| `maintain-source-registry` | Adds or changes sources, with approval for persistent changes |
| `queue-intelligence-run` | Plans a run and creates bounded work items. Drafts, validates, then queues |
| `process-work-item-result` | Reads worker results, applies status and retry rules, reports review load |
| `public-web-research` | Guidance for evidence-led public web research |
| `synthesise-output` | Builds the report from findings and evidence |
| `render-html-report` | Renders the self-contained HTML report with source links |
| `request-approval` | Creates `PublishOutput` and other approval items |
| `publish-approved-output` | Publishes only after approval |
| `draft-from-template` | Drafts a document from a registered template |
| `learn-from-run` | Captures reusable learning from a run for approval |
| `recover-second-brain` | Rebuilds second-brain structure after loss or drift |

## Skills updated after version 1.0.0.2

### `queue-intelligence-run`

- Pre-flight validation before any work item is created.
- Items are created as Draft, the batch is validated, and items are queued only as the final write. Queueing never happens at creation.
- No work item that cannot succeed is queued. This was the main cause of avoidable review load.

### `process-work-item-result`

- Retry rules: attempts 1 and 2 use Needs Remediation; Needs Review applies only after the final attempt and only for usable ambiguity; setup and system defects go to Failed with a `[SETUP]` tag.
- A review load policy classifying findings as must-review, spot-check or auto-accepted.
- Run reports state the human effort first and list must-review findings with their reason.
- Finding summaries are limited to 500 characters.

### `synthesise-output`

- Reports are saved as both `<run-id>.html` and `<run-id>.md`.
- The IO Output Artifact SharePoint URL points to the HTML file; the Markdown URL goes in the Citation Map.

See [Work item lifecycle](10-work-item-lifecycle.md) for the rules these skills implement.

## Models

| Component | Model in the reference build |
|---|---|
| Copilot Studio agent (orchestration, queueing, synthesis) | Claude Sonnet 5.5 |
| `IO Process Work Item v3`, worker step | GPT-5.6 Reasoning Luna |
| `IO Process Work Item v3`, final summary step | The published agent's model, Claude Sonnet 5.5 |

- The final summary step invokes the published agent, so it runs on the agent's model. Set the model in the agent's settings in Copilot Studio.
- The worker is an inline agent inside the workflow. Set its model in the workflow's worker step.
- If a model is unavailable in the target environment or region, choose an approved supported model and repeat the behavioural and safety tests before enabling the trigger.
- Cost and quality depend on the model and on retries. Reducing avoidable retries lowers cost more than changing model.
