# Approval notifications

The `IO Notify Approver of Report` workflow tells reviewers when a report is ready. It sends an Outlook email and posts an adaptive card to a Teams channel. It never publishes anything; approval still happens in the Control Center.

## Trigger

The workflow runs when a row is created in **IO Approval Item** that matches:

```text
cadpbm_approvaltype eq 267730000 and cadpbm_status eq 267730000
```

That is a `PublishOutput` approval in `Pending` status. Other approval types (AddEntity, AddSource, AcceptFinding and so on) do not send notifications.

## What it does

| Step | Action |
|---|---|
| 1 | Reads the output artifact, run and IO Configuration for the approval item |
| 2 | Lists the run's findings that are not Rejected or Archived |
| 3 | Splits them into must-review, Medium confidence auto-accepted and High confidence auto-accepted |
| 4 | Builds a summary and the list of flagged findings with their review reasons |
| 5 | Sends an Outlook email to the approver on the approval item plus everyone in the configuration's **Approver Email List** |
| 6 | Posts an adaptive card to the configured Teams channel |

The message leads with the review effort, for example `9 must-review, 39 Medium auto-accepted (spot-check 10-20%), 72 High auto-accepted`, so reviewers know how much work to expect before they open the report.

## Connection references

| Connection reference | Connector | Used for |
|---|---|---|
| `IO Notify - Office 365 Outlook` (`cadpbm_IONotifyOutlook`) | Office 365 Outlook | Send the review email |
| `IO Notify - Microsoft Teams` (`cadpbm_IONotifyTeams`) | Microsoft Teams | Post the adaptive card |
| IO Workflow Dataverse connection | Microsoft Dataverse | Read the approval, output, run, configuration and findings |

The sender of the email and the Teams post is the identity behind each connection. Bind them to an account or service account that approvers recognise.

## Configure before enabling

1. Bind the three connection references during import or in the solution.
2. Fill **Approver Email List** on the active IO Configuration row. Without it, only the approver on the approval item receives the email.
3. Set the Teams **team** and **channel** in the `Post_to_Teams` action. These values are specific to a tenant and must be set in every target environment.
4. Confirm DLP allows Office 365 Outlook and Microsoft Teams in the same data group as Dataverse.
5. Test with a single run and one pending `PublishOutput` approval before activating.

## Behaviour to expect

- One email and one Teams card per pending approval item.
- Notifications are informational. The report stays in `Review Required` until a reviewer approves or rejects it.
- If the email or Teams step fails, the approval item is unaffected. Check the workflow run history.
- The Teams card carries only the title, run ID, counts, flagged finding summaries and a link to the report. Do not add sensitive content to it.
