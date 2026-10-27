---
name: synthesise-output
description: Create a cited intelligence output from completed work items, using IO Findings, IO Evidence Sources and the template profile. Creates an IO Output Artifact in Review Required state.
---
<!-- bic:source=blank -->
# Synthesise Output

Use IO Findings, IO Evidence Sources and the resolved template profile to generate markdown/HTML/DOC draft content. Never publish from this skill — always leave the artefact in Review Required state.

## When to use this skill

- process-work-item-result has confirmed all work items for a run are terminal and synthesis is appropriate.
- The user explicitly asks to draft/synthesise the output for a run.

## Procedure

1. Read the IO Run and confirm its work items are all terminal (hand back to process-work-item-result if not).
2. Read the IO Template Registry row for the run's Output Template ID; refuse if Agent Ready is not Yes and explain what is missing.
3. Read all IO Finding rows for the run with Status = Ready For Synthesis (or promote well-formed Draft findings after checking them), and their linked IO Evidence Source rows.
4. Select the matching template structure below for the template's Domain Type/Template ID (or read the live template/profile from SharePoint once the second-brain read tool is available — the structures below are the shipped starter definitions and are the fallback source of truth until then).
5. Populate every required section using only findings and evidence records from the active IO Run. Mark any missing required fact as `[REVIEW: source not found]` rather than inventing it.
6. Include a Sources section listing evidence/finding citations for every material claim.
7. Add empty/starter Reviewer notes for the human reviewer to fill in.
8. Create or prepare the drafted report body and citation map. Then invoke the render-html-report skill to transform the draft into the reusable self-contained HTML report template with source links from IO Evidence Source.URL. Store the resulting HTML as the IO Output Artifact Markdown Body or HTML body according to the implementation, with Status = Review Required, Title and Artifact Type (Brief, Digest, Battlecard, Report, DraftDocument, ReviewPack or Dataset as appropriate).
9. Hand off to request-approval for a PublishOutput approval — do not publish directly from here.


## Dedupe and update classification

Before drafting the final output, the orchestrator must perform a synthesis-stage dedupe and update check. This is the broad, cross-work-item check; inline workers may still avoid obvious duplicates within a single work item.

1. Read all current-run IO Finding rows and linked IO Evidence Source rows for the Run ID.
2. Read prior IO Finding rows for the same Domain ID and, where available, same Entity ID / Theme / Finding Type from recent previous runs.
3. Compare current findings against prior findings using:
   - entity or candidate entity;
   - source URL / evidence URL;
   - theme;
   - summary / claim;
   - published date;
   - evidence IDs;
   - confidence.
4. Classify each current finding before inclusion:
   - **NewSignal**: no prior materially similar finding exists.
   - **Update**: same topic/entity as a prior finding, but materially changed, newly quantified, newly dated, or strategically different.
   - **Repeated / duplicate**: same source or same claim already appeared previously with no meaningful change.
   - **Candidate Entity**: possible new competitor/entity pending AddEntity approval.
   - **Excluded / low confidence**: unsupported, stale, irrelevant, or below confidence threshold.
5. Exclude repeated/duplicate findings from the main report body unless the template explicitly asks for repeated coverage.
6. Put excluded or repeated items in an appendix or reviewer note if useful.
7. Preserve counts for the report header/reviewer summary:
   - New items count
   - Update items count
   - Repeated/excluded count
   - Candidate entities pending approval count
   - No Material Activity count
8. Do not mutate prior findings. If reclassification is needed, write the classification in the new output/citation map or current finding status/notes according to available fields.

## Needs Review handling

Treat IO Work Items with Search Status = Needs Review as terminal for report-readiness counting. Do not block synthesis because a work item needs review.

During synthesis:

1. Include findings with Finding Status = Needs Review only in clearly labelled review areas.
2. Add a section called **Findings requiring reviewer verification** whenever any finding or source is marked Needs Review, low confidence, incomplete, ambiguous, or missing evidence.
3. For each review item, include:
   - entity/source/theme;
   - finding summary;
   - what needs verification;
   - confidence;
   - evidence/source IDs;
   - recommended reviewer action.
4. Do not present Needs Review findings as final or fully trusted.
5. Keep the IO Output Artifact in Review Required status.
6. Add a reviewer note that the report may proceed, but flagged findings require human verification before publication or downstream use.
## Template structures (starter definitions; keep in sync with the live templates/profiles once SharePoint access is configured)

Shared drafting rules for every template below:
- Use only findings and evidence records from the active IO Run.
- Mark missing facts with `[REVIEW: source not found]`.
- Include citations or source IDs for each material claim.
- Do not publish without approval.

### competitor-weekly-brief (Competitor)
Required sections: Scope statement, Emerging themes, Competitor moves by theme, M&A / exec moves, Coverage check, Sources, Reviewer notes.


### competitor-battlecard (Competitor)
Required sections: Executive snapshot, Competitor overview, Market and buyer context, Momentum tracker, Capability heatmap, Scenario-based comparison, Commercial and packaging comparison, Differentiators and watch-outs, Objection handling, Recommended actions, Sources and confidence ratings, Reviewer notes.
Drafting rules: use only evidence-backed claims, separate fact/interpretation/action, include confidence ratings, tie comparisons to customer scenarios and buying drivers, and mark missing facts with `[REVIEW: source not found]`.
### market-signal-digest (MarketCustomer)
Required sections: Executive take, Signals by segment, Customer implications, Opportunities, Risks, Sources, Reviewer notes.

### regulatory-impact-brief (RegulatoryPolicy)
Required sections: Policy change summary, Applicability, Impact assessment, Required actions, Timeline, Sources, Reviewer notes.

### account-brief (AccountStakeholder)
Required sections: Account snapshot, Recent signals, Stakeholder map, Opportunities and risks, Recommended talking points, Sources, Reviewer notes.

### pmo-status-report (PMOGovernance)
Required sections: Executive summary, Overall RAG, Progress this period, Next period focus, Risks issues dependencies, Decisions and asks, Reviewer notes.

### incident-situation-brief (RiskIncident)
Required sections: Situation summary, Current impact, Timeline, Actions taken, Open risks, Next update, Sources, Reviewer notes.

Every template also carries a header with Generated timestamp, Run ID and Scope statement.

## Guardrails

- Never invent facts, citations, approvals, dates, owners, budgets or decisions.
- Refuse to draft if the template is not Agent Ready, and explain exactly what is missing.
- Respect the Sensitivity Ceiling from IO Configuration — refuse to include above-ceiling content and flag it instead.
- Leave the artefact in Review Required state; never set it to Approved or Published from this skill.
