---
name: publish-approved-output
description: Publish approved outputs to SharePoint reports/drafts. Only runs after IO Approval Item is Approved.
---
<!-- bic:source=blank -->
# Publish Approved Output

Use only after an IO Approval Item for PublishOutput is Approved. This skill performs the actual write to the human-readable second brain.

## When to use this skill

- The user asks to publish a reviewed brief/report/draft, and its approval item shows Status = Approved.

## Procedure

1. Read the IO Approval Item and verify Status = Approved. If not Approved, stop and explain the current status instead of publishing.
2. Read the IO Output Artifact and verify Status = Approved or Review Required with a matching Approved approval item.
3. Resolve the SharePoint destination from IO Configuration (Site URL, Folder Path) and the Template Registry (Template Path / whether it goes to `docs/reports` or a Drafts location, per the template's Human Review Required and Template Type).
4. Write the output content to that destination.
5. Update the IO Output Artifact's SharePoint URL and set Status = Published.
6. If all required approvals for the parent IO Run are complete, update IO Run Status = Published.
7. Propose learned-context updates as a **separate** approval via the learn-from-run skill — never write learned context automatically as part of publishing.

## Guardrails

- Never publish without a confirmed Approved status on the relevant IO Approval Item.
- Never write to SharePoint from this skill without having verified the approval first — this is the one place in the solution that performs the actual external/second-brain write, so treat every write here as a side effect requiring the prior approval gate, not additional in-skill confirmation once approval is verified.
- Do not silently roll learned-context updates into a publish action.
