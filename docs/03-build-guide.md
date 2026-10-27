# Build guide

## Prerequisites

- Power Platform CLI 2.9.3 or later
- A Dataverse-enabled Power Platform environment
- Modern Copilot Studio with native Workflows available
- Power Apps Code Apps enabled
- SharePoint Online site for the second brain
- Permission to import solutions and configure connections
- Copilot Studio capacity through Copilot Credits or PAYG
- Power Apps Premium for Control Center users
- Microsoft 365 licences for SharePoint, Teams, and Exchange features used
- Connector or publisher licensing for restricted APIs, feeds, or data sources

## Security and DLP

- Assign least-privilege Dataverse roles to users and connection owners.
- Scope SharePoint permissions to the configured second-brain location.
- Keep credentials in managed connections or approved secret stores.
- Place Dataverse, SharePoint, Copilot Studio, and Power Apps in the same Business data group.
- Approve Outlook and Teams before enabling delivery.
- Approve RSS, HTTP, custom connectors, and external APIs explicitly.
- Apply endpoint filtering and tenant-isolation policy where required.
- Do not mix Business and Non-Business connectors in one Workflow.

## Build a local unmanaged package

From the repository root:

```powershell
pac solution pack `
  --folder .\src\solution `
  --zipfile .\dist\strategic-intelligence-operations-unmanaged-v1.0.0.8.zip `
  --packagetype Unmanaged
```

The committed managed package is produced from a managed export of the same source solution, unpacked with `--packagetype Managed`, sanitized with the same reusable defaults, and repacked with `--packagetype Managed`. PAC does not support relabelling an unmanaged source tree as managed.

Use a short drive alias if Windows path limits affect deeply nested Copilot Studio knowledge files:

```powershell
subst S: $PWD
pac solution pack --folder S:\src\solution --zipfile S:\dist\strategic-intelligence-operations-unmanaged-v1.0.0.8.zip --packagetype Unmanaged
subst S: /D
```

Do not edit generated component identifiers unless the corresponding component is intentionally being replaced.

After packing, calculate and record SHA-256 checksums:

```powershell
Get-FileHash .\dist\*.zip -Algorithm SHA256
```
