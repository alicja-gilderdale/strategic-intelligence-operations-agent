# Import and deployment guide

## Prerequisites and dependencies

### Platform and licensing

- A Microsoft Power Platform environment with Dataverse.
- Microsoft Copilot Studio entitlement and permission to create, import, configure, and publish agents.
- Power Apps permission to use the imported Control Center code app.
- Appropriate Microsoft 365 and SharePoint licensing for the second-brain workspace.

### Environment permissions

- Solution import and customization privileges.
- Permission to create and update rows in the 11 `cadpbm_io*` Dataverse tables.
- Permission to create or select connection references.
- SharePoint access to the configured site and folder.
- Permission to enable or activate the worker workflow after testing.

### Connectors and capabilities

| Dependency | Purpose | Required |
|---|---|---:|
| Microsoft Dataverse connector / MCP Server | Operational control-plane reads and writes | Yes |
| SharePoint MCP | Human-readable second-brain files | Yes for the reference implementation |
| Search all websites | Current public-web evidence and discovery | Yes for current intelligence |
| Native Copilot Studio Workflows | Asynchronous work-item processing | Yes |
| Microsoft 365 Copilot and/or Teams channel | End-user access | Optional; enable only after review |
| Office 365 Outlook and Microsoft Teams connectors | Approval notifications ([guide](09-approval-notifications.md)) | Yes, to send review notifications |

The imported package contains connection references, not source-environment credentials. Create or bind target-environment connections during import.

### DLP and governance

- Allow Dataverse, SharePoint MCP, Copilot Studio, and Power Apps connectors in the same approved DLP data group.
- Confirm tenant policy permits Search all websites for the intended sensitivity level.
- Define the SharePoint location, approvers, and sensitivity ceiling before processing data.
- Use least-privilege security roles and test in a non-production environment first.

### Model availability

| Component | Model in the reference build |
|---|---|
| Copilot Studio agent, including the workflow's final summary step | Claude Sonnet 5.5 |
| Workflow worker step | GPT-5.6 Reasoning Luna |

The final summary step invokes the published agent, so it uses the agent's model. If a model is unavailable in the target environment or region, select an approved supported model and repeat the behavioral and safety tests before publishing. See [Agent skills and models](11-agent-skills.md).

### Solution dependencies

The source solution manifest reports no unresolved Power Platform component dependencies. Runtime services and target-environment connections listed above are still required.

## Choose a package

| Package | Recommended use |
|---|---|
| `dist/strategic-intelligence-operations-managed-v1.0.0.8.zip` | Controlled test, UAT, and production deployments where components should remain governed |
| `dist/strategic-intelligence-operations-unmanaged-v1.0.0.8.zip` | Development environments where builders need to extend or inspect solution components |

## Import

1. Download the managed package for downstream deployment, or the unmanaged package for an extensible development environment.
2. Import it into a non-production Dataverse environment first.
3. Create or select the required Dataverse and SharePoint MCP connections.
4. Confirm the imported agent, workflow, tables, and Control Center app.

## Configure before enabling

1. Create the active `Default` IO Configuration row.
2. Set the SharePoint site and second-brain folder.
3. Set the sensitivity ceiling and approver list.
4. Validate the SharePoint registry structure.
5. Confirm DLP and connector permissions.
6. Review the imported agent before exposing channels.
7. Replace the disabled workflow filter with the approved production filter:

```text
cadpbm_status eq 267730001 or (cadpbm_status eq 267730006 and cadpbm_retries lt 3)
```

8. Activate the workflow only after an isolated test passes.
9. Bind the Outlook and Teams connection references, set the Teams team and channel, and activate `IO Notify Approver of Report` ([approval notifications](09-approval-notifications.md)).

Retry and status behaviour is described in [Work item lifecycle](10-work-item-lifecycle.md).

## Package selection

- **Managed:** recommended for reusable downstream installation.
- **Unmanaged:** development and controlled extension only.

Validated release ZIPs are committed under `dist` in this repository and may also be attached to tagged GitHub Releases.

## Post-import checks

- Create one test run and one queued work item.
- Confirm retries increment correctly.
- Confirm evidence and findings are written to Dataverse.
- Confirm synthesis starts only when non-discovery work items are terminal.
- Confirm output remains review-required until approved.
- Confirm no content is published externally without explicit approval.

## Known deployment notes

- The agent package may import with Microsoft 365 Copilot and Teams channels represented in its configuration. Review channel exposure before publishing.
- The agent is configured to publish on import by the source component. Confirm its publication state and channel availability immediately after import.
- The worker workflow trigger is intentionally disabled in both committed packages.
- Importing the unmanaged package into a shared environment allows direct customization; use the managed package where solution governance is required.
