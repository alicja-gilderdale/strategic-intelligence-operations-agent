# Configuration

## Required IO Configuration values

| Field | Purpose |
|---|---|
| Configuration ID | Stable workspace identifier; use `Default` for initial setup |
| Solution Name | Human-readable deployment name |
| SharePoint Site URL | Approved second-brain site |
| Folder Path | Root folder for registries, profiles, templates, and reports |
| Sensitivity Ceiling | Maximum content sensitivity the solution may process |
| Approver Email List | Reviewers authorized to approve governed changes and outputs. Also receive review notifications by email |
| Is Active | Enables the configuration for runtime use |

## SharePoint starter structure

```text
domains/
entities/
  profiles/
sources/
  profiles/
templates/
  profiles/
  template_files/
docs/
  reports/
  learned/
  decisions/
  architecture/
  governance/
```

Do not store credentials in configuration rows, SharePoint profiles, agent instructions, or repository files.
