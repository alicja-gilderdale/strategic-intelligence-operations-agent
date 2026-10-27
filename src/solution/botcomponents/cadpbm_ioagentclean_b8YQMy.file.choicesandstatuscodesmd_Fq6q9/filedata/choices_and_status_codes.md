# Choice values and status codes

Use publisher prefix `cadpbm` unless your environment requires a different prefix.

## Domain Type

- Competitor = 267730000
- MarketCustomer = 267730001
- RegulatoryPolicy = 267730002
- AccountStakeholder = 267730003
- PMOGovernance = 267730004
- RiskIncident = 267730005
- Generic = 267730006

## Work Item Search Status

- Draft = 267730000
- Queued = 267730001
- In Progress = 267730002
- Completed = 267730003
- No Material Activity = 267730004
- Needs Review = 267730005
- Needs Remediation = 267730006
- Failed = 267730007
- Skipped = 267730008

## Terminal work item states

The orchestration can continue when every work item for a run is one of:

- Completed
- No Material Activity
- Needs Review
- Failed
- Skipped

Failed items should normally block publication unless explicitly overridden by a reviewer.




## Work Item Task Type

Base task types:

- Research = 267730000
- Extract = 267730001
- Compare = 267730002
- Summarise = 267730003
- Draft = 267730004
- Validate = 267730005
- Remediate = 267730006
- Discovery = 267730007 (domain-aware pre-run discovery of new candidate entities; option value formerly labelled "Competitor Discovery")

## Finding Type

- Candidate Entity = 267730007

