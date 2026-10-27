# Export and unpack guide

Export the unmanaged solution:

```powershell
pac solution export `
  --environment "https://<environment>.crm.dynamics.com/" `
  --name "StrategicIntelligenceOperations" `
  --path .\exports\strategic-intelligence-operations-unmanaged-v1.0.0.8.zip `
  --managed false `
  --overwrite true
```

Unpack it:

```powershell
pac solution unpack `
  --zipfile .\exports\strategic-intelligence-operations-unmanaged-v1.0.0.8.zip `
  --folder .\src\solution `
  --packagetype Unmanaged `
  --clobber true
```

Raw ZIP files under `exports` are intentionally ignored. Commit unpacked source and the validated managed and unmanaged packages under `dist`. The same packages may also be attached to a tagged GitHub Release.

After each export, repeat the genericity and secret scan and confirm that the safe workflow trigger has not been replaced by a live production filter.
