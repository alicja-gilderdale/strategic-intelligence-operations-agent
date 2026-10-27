# End-to-end operating model

## Roles

- Business owner: accountable for domain scope and output quality.
- Intelligence lead: owns entity/source/template registries.
- Builder/admin: owns Copilot Studio, Dataverse, workflows and ALM.
- Reviewer/approver: approves generated outputs before publication.
- Consumer: reads briefs/drafts and provides feedback.

## Governance gates

1. Configuration exists and points to approved SharePoint folder.
2. Domain context profile exists and has been confirmed by the user.
3. Domain is active in Dataverse and the second-brain registry.
4. Entity and source profiles are active.
5. Template is active and has an approved profile.
6. Work item completes with citations/evidence.
7. Output passes human review.
8. Learned context update is approved before persistence.
