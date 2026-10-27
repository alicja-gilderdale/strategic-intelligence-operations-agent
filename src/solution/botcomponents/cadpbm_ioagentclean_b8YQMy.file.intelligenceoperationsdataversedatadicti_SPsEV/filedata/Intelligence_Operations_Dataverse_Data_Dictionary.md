# Strategic Intelligence Operations Dataverse data dictionary

Generated from the canonical schema CSV files in `01_dataverse_schema`.

Target solution: `StrategicIntelligenceOperations`
Target environment: configure during deployment.

## Overview

The Strategic Intelligence Operations schema uses Dataverse as the operational control plane for a governed, reusable intelligence agent. SharePoint remains the human-readable second brain for registries, profiles, templates, reports, learned context, and decisions.

## Table build order

| Order | Table | Purpose |
|---:|---|---|
| 1 | IO Configuration | Stores the active runtime configuration for the Intelligence Operations deployment, including SharePoint second-brain location, default domain, approver list, and sensitivity ceiling. |
| 2 | IO Domain Registry | Defines each active or planned intelligence domain, such as competitor intelligence, PMO governance drafting, or regulatory monitoring. |
| 3 | IO Template Registry | Defines approved output templates and their profiles so the agent knows which artefacts it may draft and what human review is required. |
| 4 | IO Entity Registry | Stores tracked entities within a domain, such as competitors, accounts, regulators, projects, programmes, incidents, or risk themes. |
| 5 | IO Source Registry | Stores approved information sources, source types, credibility tiers, allowed use, and lifecycle status for intelligence gathering. |
| 6 | IO Run | Represents one scheduled or ad hoc intelligence operation, including domain, period, requested output template, requester, and lifecycle status. |
| 7 | IO Work Item | Represents one bounded unit of work within a run, enabling queue-based search processing, retries, remediation, and worker status tracking. |
| 8 | IO Evidence Source | Stores source evidence collected or extracted by worker tasks, including source title, URL/location, dates, credibility, and excerpt. |
| 9 | IO Finding | Stores structured findings generated from work items, linked to evidence and ready for synthesis into outputs. |
| 10 | IO Output Artifact | Stores generated draft or published intelligence outputs, including markdown body, citation map, SharePoint URL, and review/publish status. |
| 11 | IO Approval Item | Stores human approval requests for publishing outputs, updating the second brain, or accepting/rejecting findings or registry changes. |

## Relationship summary

| From table | Column | To table | Purpose |
|---|---|---|---|
| IO Approval Item | Output ID | IO Output Artifact | Output being approved |
| IO Approval Item | Finding ID | IO Finding | Finding being approved |
| IO Domain Registry | Configuration ID | IO Configuration | Configuration/workspace that owns this domain |
| IO Entity Registry | Domain ID | IO Domain Registry | Owning intelligence domain |
| IO Evidence Source | Work Item ID | IO Work Item | Parent work item |
| IO Finding | Run ID | IO Run | Parent run |
| IO Finding | Work Item ID | IO Work Item | Parent work item |
| IO Finding | Entity ID | IO Entity Registry | Related entity |
| IO Output Artifact | Run ID | IO Run | Parent run |
| IO Output Artifact | Template ID | IO Template Registry | Template used |
| IO Run | Configuration ID | IO Configuration | Configuration selected for this run |
| IO Run | Domain ID | IO Domain Registry | Domain for run |
| IO Run | Output Template ID | IO Template Registry | Template to use |
| IO Source Registry | Domain ID | IO Domain Registry | Owning domain |
| IO Work Item | Run ID | IO Run | Parent run |
| IO Work Item | Entity ID | IO Entity Registry | Target entity |
| IO Work Item | Source ID | IO Source Registry | Target source |

## Table definitions

### IO Configuration

Stores the active runtime configuration for the Intelligence Operations deployment, including SharePoint second-brain location, default domain, approver list, and sensitivity ceiling.

| Property | Value |
|---|---|
| Display name | IO Configuration |
| Schema name | `cadpbm_IOConfiguration` |
| Logical name | `cadpbm_ioconfiguration` |
| Primary name column | Configuration ID (`cadpbm_configurationid`) |

#### Columns

| Column | Schema / logical name | Type | Required | Primary | Choices / relationship | Description |
|---|---|---|---|---|---|---|
| Configuration ID | `cadpbm_ConfigurationID`<br>`cadpbm_configurationid` | Text | Required | Yes |  | Use Default for the primary runtime row |
| Solution Name | `cadpbm_SolutionName`<br>`cadpbm_solutionname` | Text | Required | No |  | Display name for this deployment |
| SharePoint Site URL | `cadpbm_SharePointSiteURL`<br>`cadpbm_sharepointsiteurl` | URL | Required | No |  | Root site containing the second brain |
| Folder Path | `cadpbm_FolderPath`<br>`cadpbm_folderpath` | Text | Required | No |  | Root folder path under the site |
| Default Domain | `cadpbm_DefaultDomain`<br>`cadpbm_defaultdomain` | Choice | Optional | No | Competitor=267730000<br>MarketCustomer=267730001<br>RegulatoryPolicy=267730002<br>AccountStakeholder=267730003<br>PMOGovernance=267730004<br>RiskIncident=267730005 | Default operating domain |
| Approver Email List | `cadpbm_ApproverEmailList`<br>`cadpbm_approveremaillist` | Multiline Text | Optional | No |  | Semicolon separated approvers |
| Sensitivity Ceiling | `cadpbm_SensitivityCeiling`<br>`cadpbm_sensitivityceiling` | Choice | Required | No | Public=267730000<br>General=267730001<br>Confidential=267730002<br>HighlyConfidential=267730003 | Maximum allowed content classification |
| Is Active | `cadpbm_IsActive`<br>`cadpbm_isactive` | Two Options | Required | No | No=0<br>Yes=1 | Only one Default active row should be used |

#### Length conventions

- Multiline text columns are configured for 2,000 characters.
- URL columns are configured for 500 characters.

### IO Domain Registry

Defines each active or planned intelligence domain, such as competitor intelligence, PMO governance drafting, or regulatory monitoring.

| Property | Value |
|---|---|
| Display name | IO Domain Registry |
| Schema name | `cadpbm_IODomainRegistry` |
| Logical name | `cadpbm_iodomainregistry` |
| Primary name column | Domain ID (`cadpbm_domainid`) |

#### Columns

| Column | Schema / logical name | Type | Required | Primary | Choices / relationship | Description |
|---|---|---|---|---|---|---|
| Domain ID | `cadpbm_DomainID`<br>`cadpbm_domainid` | Text | Required | Yes |  | Stable ID such as competitor-intelligence |
| Configuration ID | `cadpbm_ConfigurationID`<br>`cadpbm_configurationid` | Lookup | Required | No | Lookup to IO Configuration | Configuration/workspace that owns this domain |
| Domain Name | `cadpbm_DomainName`<br>`cadpbm_domainname` | Text | Required | No |  | Human display name |
| Domain Type | `cadpbm_DomainType`<br>`cadpbm_domaintype` | Choice | Required | No | Competitor=267730000<br>MarketCustomer=267730001<br>RegulatoryPolicy=267730002<br>AccountStakeholder=267730003<br>PMOGovernance=267730004<br>RiskIncident=267730005 | Controls allowed templates and source types |
| Domain Status | `cadpbm_Status`<br>`cadpbm_status` | Choice | Required | No | Draft=267730000<br>Active=267730001<br>Paused=267730002<br>Retired=267730003 | Only Active domains can be used for runs |
| Second Brain Path | `cadpbm_SecondBrainPath`<br>`cadpbm_secondbrainpath` | Text | Required | No |  | Folder path for domain-specific files |
| Owner Email | `cadpbm_OwnerEmail`<br>`cadpbm_owneremail` | Email | Optional | No |  | Business owner |

### IO Template Registry

Defines approved output templates and their profiles so the agent knows which artefacts it may draft and what human review is required.

| Property | Value |
|---|---|
| Display name | IO Template Registry |
| Schema name | `cadpbm_IOTemplateRegistry` |
| Logical name | `cadpbm_iotemplateregistry` |
| Primary name column | Template ID (`cadpbm_templateid`) |

#### Columns

| Column | Schema / logical name | Type | Required | Primary | Choices / relationship | Description |
|---|---|---|---|---|---|---|
| Template ID | `cadpbm_TemplateID`<br>`cadpbm_templateid` | Text | Required | Yes |  | Stable template ID |
| Domain Type | `cadpbm_DomainType`<br>`cadpbm_domaintype` | Choice | Required | No | Competitor=267730000<br>MarketCustomer=267730001<br>RegulatoryPolicy=267730002<br>AccountStakeholder=267730003<br>PMOGovernance=267730004<br>RiskIncident=267730005<br>Generic=267730006 | Allowed domain |
| Template Name | `cadpbm_TemplateName`<br>`cadpbm_templatename` | Text | Required | No |  | Display name |
| Template Type | `cadpbm_TemplateType`<br>`cadpbm_templatetype` | Choice | Required | No | Markdown=267730000<br>HTML=267730001<br>DOCX=267730002<br>PPTX=267730003<br>CSV=267730004 | Template file type |
| Template Path | `cadpbm_TemplatePath`<br>`cadpbm_templatepath` | Text | Required | No |  | SharePoint path |
| Profile Path | `cadpbm_ProfilePath`<br>`cadpbm_profilepath` | Text | Required | No |  | Template profile path |
| Agent Ready | `cadpbm_AgentReady`<br>`cadpbm_agentready` | Two Options | Required | No | No=0<br>Yes=1 | Only ready templates can be used |
| Human Review Required | `cadpbm_HumanReviewRequired`<br>`cadpbm_humanreviewrequired` | Two Options | Required | No | No=0<br>Yes=1 | Default should be Yes |

### IO Entity Registry

Stores tracked entities within a domain, such as competitors, accounts, regulators, projects, programmes, incidents, or risk themes.

| Property | Value |
|---|---|
| Display name | IO Entity Registry |
| Schema name | `cadpbm_IOEntityRegistry` |
| Logical name | `cadpbm_ioentityregistry` |
| Primary name column | Entity ID (`cadpbm_entityid`) |

#### Columns

| Column | Schema / logical name | Type | Required | Primary | Choices / relationship | Description |
|---|---|---|---|---|---|---|
| Entity ID | `cadpbm_EntityID`<br>`cadpbm_entityid` | Text | Required | Yes |  | Stable ID for competitor account project regulator etc |
| Domain ID | `cadpbm_DomainID`<br>`cadpbm_domainid` | Lookup | Required | No | Lookup to IO Domain Registry | Owning intelligence domain |
| Entity Name | `cadpbm_EntityName`<br>`cadpbm_entityname` | Text | Required | No |  | Human name |
| Entity Type | `cadpbm_EntityType`<br>`cadpbm_entitytype` | Choice | Required | No | Competitor=267730000<br>Customer=267730001<br>MarketSegment=267730002<br>Regulator=267730003<br>PolicyTopic=267730004<br>Account=267730005<br>Stakeholder=267730006<br>Project=267730007<br>Programme=267730008<br>RiskTheme=267730009<br>Incident=267730010 | Generic entity category |
| Profile Path | `cadpbm_ProfilePath`<br>`cadpbm_profilepath` | Text | Optional | No |  | SharePoint markdown profile path |
| Entity Status | `cadpbm_Status`<br>`cadpbm_status` | Choice | Required | No | Active=267730000<br>Watchlist=267730001<br>Paused=267730002<br>Retired=267730003 | Controls task generation |
| Priority | `cadpbm_Priority`<br>`cadpbm_priority` | Choice | Optional | No | Low=267730000<br>Medium=267730001<br>High=267730002<br>Critical=267730003 | Queue weighting |

### IO Source Registry

Stores approved information sources, source types, credibility tiers, allowed use, and lifecycle status for intelligence gathering.

| Property | Value |
|---|---|
| Display name | IO Source Registry |
| Schema name | `cadpbm_IOSourceRegistry` |
| Logical name | `cadpbm_iosourceregistry` |
| Primary name column | Source ID (`cadpbm_sourceid`) |

#### Columns

| Column | Schema / logical name | Type | Required | Primary | Choices / relationship | Description |
|---|---|---|---|---|---|---|
| Source ID | `cadpbm_SourceID`<br>`cadpbm_sourceid` | Text | Required | Yes |  | Stable source ID |
| Domain ID | `cadpbm_DomainID`<br>`cadpbm_domainid` | Lookup | Required | No | Lookup to IO Domain Registry | Owning domain |
| Source Name | `cadpbm_SourceName`<br>`cadpbm_sourcename` | Text | Required | No |  | Display name |
| Source Type | `cadpbm_SourceType`<br>`cadpbm_sourcetype` | Choice | Required | No | PublicWeb=267730000<br>SharePoint=267730001<br>M365Meeting=267730002<br>M365Email=267730003<br>TeamsChat=267730004<br>CRM=267730005<br>Dataverse=267730006<br>ManualUpload=267730007<br>AzureBlob=267730008 | Controls tool routing |
| Source URL | `cadpbm_SourceURL`<br>`cadpbm_sourceurl` | URL | Optional | No |  | Base URL or location |
| Allowed Use | `cadpbm_AllowedUse`<br>`cadpbm_alloweduse` | Choice | Required | No | ReadOnly=267730000<br>ReadWriteDrafts=267730001<br>ApprovalRequired=267730002<br>Disabled=267730003 | Controls side effects |
| Credibility Tier | `cadpbm_CredibilityTier`<br>`cadpbm_credibilitytier` | Choice | Optional | No | Primary=267730000<br>TrustedSecondary=267730001<br>AnalystProvided=267730002<br>Unverified=267730003 | Used in citation weighting |
| Source Status | `cadpbm_Status`<br>`cadpbm_status` | Choice | Required | No | Active=267730000<br>Needs Review=267730001<br>Paused=267730002<br>Retired=267730003 | Only Active sources used automatically |

#### Length conventions

- URL columns are configured for 500 characters.

### IO Run

Represents one scheduled or ad hoc intelligence operation, including domain, period, requested output template, requester, and lifecycle status.

| Property | Value |
|---|---|
| Display name | IO Run |
| Schema name | `cadpbm_IORun` |
| Logical name | `cadpbm_iorun` |
| Primary name column | Run ID (`cadpbm_runid`) |

#### Columns

| Column | Schema / logical name | Type | Required | Primary | Choices / relationship | Description |
|---|---|---|---|---|---|---|
| Run ID | `cadpbm_RunID`<br>`cadpbm_runid` | Text | Required | Yes |  | Unique run ID |
| Configuration ID | `cadpbm_ConfigurationID`<br>`cadpbm_configurationid` | Lookup | Required | No | Lookup to IO Configuration | Configuration selected for this run |
| Domain ID | `cadpbm_DomainID`<br>`cadpbm_domainid` | Lookup | Required | No | Lookup to IO Domain Registry | Domain for run |
| Run Type | `cadpbm_RunType`<br>`cadpbm_runtype` | Choice | Required | No | Scheduled=267730000<br>AdHoc=267730001<br>Remediation=267730002<br>TemplateDraft=267730003 | How run started |
| Run Status | `cadpbm_Status`<br>`cadpbm_status` | Choice | Required | No | Draft=267730000<br>Queued=267730001<br>Running=267730002<br>Synthesising=267730003<br>Review Required=267730004<br>Approved=267730005<br>Published=267730006<br>Failed=267730007<br>Cancelled=267730008 | Run lifecycle |
| Requested By | `cadpbm_RequestedBy`<br>`cadpbm_requestedby` | Email | Optional | No |  | Requester email |
| Period Start | `cadpbm_PeriodStart`<br>`cadpbm_periodstart` | Date and Time | Optional | No |  | Coverage window start |
| Period End | `cadpbm_PeriodEnd`<br>`cadpbm_periodend` | Date and Time | Optional | No |  | Coverage window end |
| Output Template ID | `cadpbm_OutputTemplateID`<br>`cadpbm_outputtemplateid` | Lookup | Optional | No | Lookup to IO Template Registry | Template to use |

### IO Work Item

Represents one bounded unit of work within a run, enabling queue-based search processing, retries, remediation, and worker status tracking.

| Property | Value |
|---|---|
| Display name | IO Work Item |
| Schema name | `cadpbm_IOWorkItem` |
| Logical name | `cadpbm_ioworkitem` |
| Primary name column | Work Item ID (`cadpbm_workitemid`) |

#### Columns

| Column | Schema / logical name | Type | Required | Primary | Choices / relationship | Description |
|---|---|---|---|---|---|---|
| Work Item ID | `cadpbm_WorkItemID`<br>`cadpbm_workitemid` | Text | Required | Yes |  | Unique work item ID |
| Run ID | `cadpbm_RunID`<br>`cadpbm_runid` | Lookup | Required | No | Lookup to IO Run | Parent run |
| Entity ID | `cadpbm_EntityID`<br>`cadpbm_entityid` | Lookup | Optional | No | Lookup to IO Entity Registry | Target entity |
| Source ID | `cadpbm_SourceID`<br>`cadpbm_sourceid` | Lookup | Optional | No | Lookup to IO Source Registry | Target source |
| Task Type | `cadpbm_TaskType`<br>`cadpbm_tasktype` | Choice | Required | No | Research=267730000<br>Extract=267730001<br>Compare=267730002<br>Summarise=267730003<br>Draft=267730004<br>Validate=267730005<br>Remediate=267730006<br>Competitor Discovery=267730007 | Worker route |
| Search Prompt | `cadpbm_SearchPrompt`<br>`cadpbm_searchprompt` | Multiline Text | Required | No |  | Bounded task prompt |
| Search Status | `cadpbm_Status`<br>`cadpbm_status` | Choice | Required | No | Draft=267730000<br>Queued=267730001<br>In Progress=267730002<br>Completed=267730003<br>No Material Activity=267730004<br>Needs Review=267730005<br>Needs Remediation=267730006<br>Failed=267730007<br>Skipped=267730008 | Search/research lifecycle for this bounded work item |
| Retries | `cadpbm_Retries`<br>`cadpbm_retries` | Whole Number | Optional | No |  | Incremented by workflow |
| Worker Notes | `cadpbm_WorkerNotes`<br>`cadpbm_workernotes` | Multiline Text | Optional | No |  | Error/remediation notes |

#### Length conventions

- Multiline text columns are configured for 2,000 characters.

### IO Evidence Source

Stores source evidence collected or extracted by worker tasks, including source title, URL/location, dates, credibility, and excerpt.

| Property | Value |
|---|---|
| Display name | IO Evidence Source |
| Schema name | `cadpbm_IOEvidenceSource` |
| Logical name | `cadpbm_ioevidencesource` |
| Primary name column | Evidence ID (`cadpbm_evidenceid`) |

#### Columns

| Column | Schema / logical name | Type | Required | Primary | Choices / relationship | Description |
|---|---|---|---|---|---|---|
| Evidence ID | `cadpbm_EvidenceID`<br>`cadpbm_evidenceid` | Text | Required | Yes |  | Unique evidence ID |
| Work Item ID | `cadpbm_WorkItemID`<br>`cadpbm_workitemid` | Lookup | Required | No | Lookup to IO Work Item | Parent work item |
| Title | `cadpbm_Title`<br>`cadpbm_title` | Text | Required | No |  | Source title |
| URL | `cadpbm_URL`<br>`cadpbm_url` | URL | Optional | No |  | Source URL where applicable |
| Published Date | `cadpbm_PublishedDate`<br>`cadpbm_publisheddate` | Date and Time | Optional | No |  | Published date |
| Retrieved Date | `cadpbm_RetrievedDate`<br>`cadpbm_retrieveddate` | Date and Time | Required | No |  | Retrieval date |
| Source Type | `cadpbm_SourceType`<br>`cadpbm_sourcetype` | Choice | Required | No | PublicWeb=267730000<br>SharePoint=267730001<br>M365=267730002<br>CRM=267730003<br>Manual=267730004 | Evidence origin |
| Credibility | `cadpbm_Credibility`<br>`cadpbm_credibility` | Choice | Optional | No | High=267730000<br>Medium=267730001<br>Low=267730002<br>Unverified=267730003 | Reviewer signal |
| Excerpt | `cadpbm_Excerpt`<br>`cadpbm_excerpt` | Multiline Text | Optional | No |  | Short quoted/paraphrased support only |

#### Length conventions

- Multiline text columns are configured for 2,000 characters.
- URL columns are configured for 500 characters.

### IO Finding

Stores structured findings generated from work items, linked to evidence and ready for synthesis into outputs.

| Property | Value |
|---|---|
| Display name | IO Finding |
| Schema name | `cadpbm_IOFinding` |
| Logical name | `cadpbm_iofinding` |
| Primary name column | Finding ID (`cadpbm_findingid`) |

#### Columns

| Column | Schema / logical name | Type | Required | Primary | Choices / relationship | Description |
|---|---|---|---|---|---|---|
| Finding ID | `cadpbm_FindingID`<br>`cadpbm_findingid` | Text | Required | Yes |  | Unique finding ID |
| Run ID | `cadpbm_RunID`<br>`cadpbm_runid` | Lookup | Required | No | Lookup to IO Run | Parent run |
| Work Item ID | `cadpbm_WorkItemID`<br>`cadpbm_workitemid` | Lookup | Required | No | Lookup to IO Work Item | Parent work item |
| Entity ID | `cadpbm_EntityID`<br>`cadpbm_entityid` | Lookup | Optional | No | Lookup to IO Entity Registry | Related entity |
| Theme | `cadpbm_Theme`<br>`cadpbm_theme` | Text | Optional | No |  | Theme/category |
| Finding Type | `cadpbm_FindingType`<br>`cadpbm_findingtype` | Choice | Required | No | NewSignal=267730000<br>Update=267730001<br>Risk=267730002<br>Opportunity=267730003<br>Decision=267730004<br>Action=267730005<br>No Material Activity=267730006<br>Candidate Entity=267730007 | Finding category |
| Summary | `cadpbm_Summary`<br>`cadpbm_summary` | Multiline Text | Required | No |  | Concise finding statement. Max length 5,000 characters |
| Implication | `cadpbm_Implication`<br>`cadpbm_implication` | Multiline Text | Optional | No |  | Why it matters. Max length 5,000 characters |
| Confidence | `cadpbm_Confidence`<br>`cadpbm_confidence` | Choice | Required | No | High=267730000<br>Medium=267730001<br>Low=267730002 | Confidence level |
| Evidence IDs | `cadpbm_EvidenceIDs`<br>`cadpbm_evidenceids` | Multiline Text | Optional | No |  | Delimited evidence references |
| Finding Status | `cadpbm_Status`<br>`cadpbm_status` | Choice | Required | No | Draft=267730000<br>Ready For Synthesis=267730001<br>Needs Review=267730002<br>Rejected=267730003<br>Archived=267730004 | Finding lifecycle |

#### Length conventions

- Multiline text columns are configured for 2,000 characters.
- IO Finding Summary and Implication are configured for 5,000 characters because they may carry richer evidence-backed report and battlecard text.

### IO Output Artifact

Stores generated draft or published intelligence outputs, including markdown body, citation map, SharePoint URL, and review/publish status.

| Property | Value |
|---|---|
| Display name | IO Output Artifact |
| Schema name | `cadpbm_IOOutputArtifact` |
| Logical name | `cadpbm_iooutputartifact` |
| Primary name column | Output ID (`cadpbm_outputid`) |

#### Columns

| Column | Schema / logical name | Type | Required | Primary | Choices / relationship | Description |
|---|---|---|---|---|---|---|
| Output ID | `cadpbm_OutputID`<br>`cadpbm_outputid` | Text | Required | Yes |  | Unique output ID |
| Run ID | `cadpbm_RunID`<br>`cadpbm_runid` | Lookup | Required | No | Lookup to IO Run | Parent run |
| Template ID | `cadpbm_TemplateID`<br>`cadpbm_templateid` | Lookup | Optional | No | Lookup to IO Template Registry | Template used |
| Title | `cadpbm_Title`<br>`cadpbm_title` | Text | Required | No |  | Output title |
| Artifact Type | `cadpbm_ArtifactType`<br>`cadpbm_artifacttype` | Choice | Required | No | Brief=267730000<br>Digest=267730001<br>Report=267730002<br>DraftDocument=267730003<br>ReviewPack=267730004<br>Dataset=267730005 | Output category |
| Output Status | `cadpbm_Status`<br>`cadpbm_status` | Choice | Required | No | Draft=267730000<br>Review Required=267730001<br>Approved=267730002<br>Published=267730003<br>Rejected=267730004 | Output lifecycle |
| SharePoint URL | `cadpbm_SharePointURL`<br>`cadpbm_sharepointurl` | URL | Optional | No |  | Published or draft URL |
| Markdown Body | `cadpbm_MarkdownBody`<br>`cadpbm_markdownbody` | Multiline Text | Optional | No |  | Generated content |
| Citation Map | `cadpbm_CitationMap`<br>`cadpbm_citationmap` | Multiline Text | Optional | No |  | JSON/text citation map |

#### Length conventions

- Multiline text columns are configured for 2,000 characters.
- URL columns are configured for 500 characters.

### IO Approval Item

Stores human approval requests for publishing outputs, updating the second brain, or accepting/rejecting findings or registry changes.

| Property | Value |
|---|---|
| Display name | IO Approval Item |
| Schema name | `cadpbm_IOApprovalItem` |
| Logical name | `cadpbm_ioapprovalitem` |
| Primary name column | Approval ID (`cadpbm_approvalid`) |

#### Columns

| Column | Schema / logical name | Type | Required | Primary | Choices / relationship | Description |
|---|---|---|---|---|---|---|
| Approval ID | `cadpbm_ApprovalID`<br>`cadpbm_approvalid` | Text | Required | Yes |  | Unique approval item ID |
| Output ID | `cadpbm_OutputID`<br>`cadpbm_outputid` | Lookup | Optional | No | Lookup to IO Output Artifact | Output being approved |
| Finding ID | `cadpbm_FindingID`<br>`cadpbm_findingid` | Lookup | Optional | No | Lookup to IO Finding | Finding being approved |
| Approval Type | `cadpbm_ApprovalType`<br>`cadpbm_approvaltype` | Choice | Required | No | PublishOutput=267730000<br>UpdateSecondBrain=267730001<br>AddEntity=267730002<br>AddSource=267730003<br>AcceptFinding=267730004<br>RejectFinding=267730005 | Approval action |
| Approval Status | `cadpbm_Status`<br>`cadpbm_status` | Choice | Required | No | Pending=267730000<br>Approved=267730001<br>Rejected=267730002<br>NeedsChanges=267730003<br>Expired=267730004 | Approval lifecycle |
| Approver Email | `cadpbm_ApproverEmail`<br>`cadpbm_approveremail` | Email | Required | No |  | Assigned approver |
| Requested Change | `cadpbm_RequestedChange`<br>`cadpbm_requestedchange` | Multiline Text | Required | No |  | Exact proposed change |
| Decision Notes | `cadpbm_DecisionNotes`<br>`cadpbm_decisionnotes` | Multiline Text | Optional | No |  | Approver notes |

#### Length conventions

- Multiline text columns are configured for 2,000 characters.

## Notes for builders

- Table-specific status labels are used for custom status columns to avoid confusion with Dataverse system Status/Status Reason columns.
- Tables and columns were created in Dataverse using the Web API and `MSCRM.SolutionUniqueName: IntelligenceOperationsSolution`.
- Lookup relationships were created through `RelationshipDefinitions` with deep-inserted lookup attributes.
- Human-readable descriptions come from the schema CSV `notes` column and are also applied to Dataverse column metadata.
