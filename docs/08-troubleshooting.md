# Troubleshooting

## PAC unpack fails on a long path

Map the repository to a temporary drive letter and run pack or unpack from that shorter path. Remove the mapping afterward.

## Workflow does not start

The repository intentionally contains a disabled trigger filter. Complete deployment configuration and replace it with the approved production filter before activation.

## Agent cannot access Dataverse or SharePoint

Check connection references, user permissions, DLP policy, and the active IO Configuration row. Imported connection references do not carry source-environment credentials.

## Synthesis never starts

Confirm all non-discovery work items are in a terminal status and that the workflow terminal-count query uses the correct `cadpbm_status` logical name.

## Many work items end in Needs Review

Needs Review applies only after the final attempt and only to usable findings that need a human decision. If many items reach it, check Worker Notes for the real error and confirm each work item has an Entity ID, a Source ID where required and a bounded Search Prompt. See [Work item lifecycle](10-work-item-lifecycle.md).

## A run is stuck in Queued after every work item finished

Synthesis starts only when all non-discovery work items are terminal. Check for items still Queued, In Progress or Needs Remediation with fewer than three retries.

## No approval email or Teams card arrives

Confirm `IO Notify Approver of Report` is on, the Outlook and Teams connection references are bound, the approval is a `PublishOutput` in Pending status, the Approver Email List is filled and the Teams team and channel are set for this tenant. See [Approval notifications](09-approval-notifications.md).

## Import fails on a missing dependency or an unused connection reference

Remove flows, apps or agents in the target environment that still use an old reference, then import again. Remove connection references from the source solution that no flow or agent uses before exporting.

## The report link opens the wrong file or returns not found

The IO Output Artifact SharePoint URL must point to the `.html` report under the configured folder path. If the second-brain folder moves, update the configuration's Folder Path and the stored report URLs.

## Current intelligence has no citations

Confirm public web grounding is enabled for the worker. Do not describe results as current or latest without live research and source URLs.
