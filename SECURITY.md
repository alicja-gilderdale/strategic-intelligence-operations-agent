# Security

## Reporting

Do not open public issues containing credentials, tenant identifiers, customer data, or security-sensitive configuration. Report security concerns privately to the maintainer through the contact details on the GitHub profile, or use GitHub private vulnerability reporting if it is enabled.

## Deployment expectations

- Import into a non-production environment first.
- Configure connection references locally.
- Apply least-privilege access to Dataverse and SharePoint.
- Review DLP policy before enabling connectors.
- Set and enforce a sensitivity ceiling.
- Keep the worker workflow disabled until configuration and isolated testing are complete.
- Never commit credentials, tokens, connection secrets, or customer records.
