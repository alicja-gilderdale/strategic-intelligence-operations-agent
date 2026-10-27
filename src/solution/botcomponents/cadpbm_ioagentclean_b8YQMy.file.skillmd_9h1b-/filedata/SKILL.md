---
name: request-approval
description: Create approval items for publish, second-brain update or registry changes. Present exact proposed change and recipients before triggering any outbound notification.
---
<!-- bic:source=blank -->
# Request Approval

Use whenever another skill needs a human decision before a persistent or outbound action: publishing an output, updating the second brain, adding an entity/source, or accepting/rejecting a finding.

## When to use this skill

- synthesise-output has produced a Review Required output that needs a PublishOutput approval.
- maintain-entity-registry / maintain-source-registry needs an AddEntity/AddSource approval.
- learn-from-run needs an UpdateSecondBrain approval.
- A finding needs an AcceptFinding/RejectFinding decision.

## Procedure

1. Read the Approver Email List from IO Configuration.
2. Determine the Approval Type (PublishOutput, UpdateSecondBrain, AddEntity, AddSource, AcceptFinding or RejectFinding) and the linked record (Output ID and/or Finding ID).
3. Create the IO Approval Item with Status = Pending, the linked record(s), Approval Type and Approver Email.
4. Present the **exact** recipient list and the **exact** proposed content or change to the requesting user, and get their explicit confirmation before sending any outbound approval notification (email/Teams message).
5. On approval, update the Approval Item Status = Approved and record Decision Notes; tell the calling context (publish-approved-output, learn-from-run, maintain-entity-registry, maintain-source-registry) that it may proceed.
6. On rejection, update Status = Rejected, record Decision Notes, and do not publish or update the second brain.
7. On "needs changes", update Status = NeedsChanges and route back to the originating skill for revision before re-requesting approval.

## Privacy rule

Never include private calendar, email, chat or file details in outbound approval notifications unless the user explicitly confirms the exact message content first.

## Guardrails

- Never send an approval notification without first showing the user the exact recipients and exact content and getting explicit confirmation.
- Never treat silence or ambiguity as approval — only an explicit Approved decision unblocks downstream publishing or writes.
- Keep one Approval Item per decision needed; do not bundle unrelated changes into a single approval unless the user asks for a combined review.
