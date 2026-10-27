# Changelog

## Unreleased

- Added sanitized managed and unmanaged v1.0.0.8 solution packages in `dist`, replacing v1.0.0.2. Includes the `IO Notify Approver of Report` workflow, updated agent skills and the review load policy.
- Package sanitization: removed the source-environment name and ID from embedded agent knowledge, replaced the Teams team and channel IDs in the notification workflow with placeholders, and reset the worker trigger filter to `cadpbm_workitemid eq '__CONFIGURE_BEFORE_ENABLE__'`.
- Known: the packages still contain two unused connection references (`cr652_competitoranalystreporting…` and 
ew_sharedagentnode_2deec84f`); remove them in a later export.

- Documented the approval notification workflow (Outlook email and Teams card) and its connection references.
- Documented work item statuses, retry rules and the review load policy.
- Documented the updated `queue-intelligence-run`, `process-work-item-result` and `synthesise-output` skills.
- Documented models: agent and final summary on Claude Sonnet 5.5, worker on GPT-5.6 Reasoning Luna.
- Added troubleshooting for review load, stuck runs, notifications, import dependencies and report links.

- Prepared a reusable repository from unmanaged solution `StrategicIntelligenceOperations` version `1.0.0.2`.
- Added unpacked Power Platform source, documentation, and architecture assets.
- Removed the source-environment reference from embedded agent knowledge.
- Disabled the asynchronous worker trigger pending deployment configuration.
- Added validated managed and unmanaged v1.0.0.2 solution packages.
- Documented runtime dependencies, DLP requirements, import choices, and post-import checks.
