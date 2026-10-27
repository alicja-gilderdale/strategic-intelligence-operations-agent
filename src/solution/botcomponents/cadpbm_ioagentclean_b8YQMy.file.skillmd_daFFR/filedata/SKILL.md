---
name: draft-from-template
description: Draft a template-based artefact such as PMO status, account brief, competitor battlecard, or incident brief. Reads templates/registry.md and the selected profile, refuses if Agent Ready is not Yes, and only ever drafts — never approves or submits.
---
<!-- bic:source=blank -->
# Draft From Template

Use when the user wants a template-based artefact drafted directly (e.g. "draft this week's PMO status report") using existing findings/inputs, without necessarily queuing a brand-new full intelligence run. This skill only drafts; it never approves, publishes or submits.

## When to use this skill

- The user names a specific template/artefact to draft and has findings, evidence or source material already available (from a prior run, or supplied directly).
- Distinguish from synthesise-output: use synthesise-output when finishing a queued IO Run's work items; use this skill when the user wants a one-off draft directly against a template, e.g. drafting from documents/transcripts the user hands over now.

## Procedure

1. Read `templates/registry.md` (or the IO Template Registry) for the requested Template ID and read its profile.
2. Check Agent Ready = Yes. If not, **refuse** to draft and explain exactly what is missing (e.g. profile incomplete, template path missing) rather than attempting a best-effort draft.
3. Confirm the required inputs from the profile are available: domain and run/context ID, reporting period or incident window, relevant entities/sources, findings with evidence IDs (or user-supplied material), and the intended reviewer/approver.
4. Use the matching template structure (see below, or the live template file once SharePoint read access is configured) to populate every required section.
5. Follow the template's drafting rules: use only findings/evidence from the active context; mark missing facts with `[REVIEW: source not found]`; include citations or source IDs for every material claim; never publish without approval.
6. Present the draft to the user clearly labelled as a draft, and hand off to request-approval if the user wants to move it toward publication — do not approve or submit it yourself.

## Template structures (starter definitions; keep in sync with the live templates/profiles once SharePoint access is configured)

### account-brief (AccountStakeholder)
Required sections: Account snapshot, Recent signals, Stakeholder map, Opportunities and risks, Recommended talking points, Sources, Reviewer notes.

### competitor-weekly-brief (Competitor)
Required sections: Scope statement, Emerging themes, Competitor moves by theme, M&A / exec moves, Coverage check, Sources, Reviewer notes.


### competitor-battlecard (Competitor)
Required sections: Executive snapshot, Competitor overview, Market and buyer context, Momentum tracker, Capability heatmap, Scenario-based comparison, Commercial and packaging comparison, Differentiators and watch-outs, Objection handling, Recommended actions, Sources and confidence ratings, Reviewer notes.
Drafting rules: use only evidence-backed claims, separate fact/interpretation/action, include confidence ratings, tie comparisons to customer scenarios and buying drivers, and mark missing facts with `[REVIEW: source not found]`.
### market-signal-digest (MarketCustomer)
Required sections: Executive take, Signals by segment, Customer implications, Opportunities, Risks, Sources, Reviewer notes.

### regulatory-impact-brief (RegulatoryPolicy)
Required sections: Policy change summary, Applicability, Impact assessment, Required actions, Timeline, Sources, Reviewer notes.

### pmo-status-report (PMOGovernance)
Required sections: Executive summary, Overall RAG, Progress this period, Next period focus, Risks issues dependencies, Decisions and asks, Reviewer notes.

### incident-situation-brief (RiskIncident)
Required sections: Situation summary, Current impact, Timeline, Actions taken, Open risks, Next update, Sources, Reviewer notes.

Every template carries a header with Generated timestamp, Run ID (or context ID) and Scope statement.

## Guardrails

- Refuse to draft if Agent Ready is not Yes — do not attempt a workaround.
- Never invent facts, citations, approvals, dates, owners, budgets or decisions.
- Never approve, publish or submit from this skill — drafting only.
- Respect the Sensitivity Ceiling from IO Configuration.
