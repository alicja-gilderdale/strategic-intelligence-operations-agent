---
name: "render-html-report"
description: "Render a completed Intelligence Operations output artifact into the reusable HTML report template with direct source links and domain-specific layout labels."
---

# Render HTML Report

Use this skill after research is complete and the orchestrator has synthesized a draft report body and citation map.

## Purpose

Render a customer-agnostic, polished HTML report using the reusable Intelligence Operations HTML template. The same template framework supports competitor briefs, battlecards, market digests, regulatory briefs, account briefs, PMO status reports and incident briefs.

## Required inputs

- IO Run
- IO Output Artifact
- IO Template Registry row
- IO Findings used in the output
- IO Evidence Source rows linked through citation map / Evidence IDs
- Domain context profile
- Template profile

## Required template files

- `templates/html/io-report-html-template.html`
- `templates/html/layouts/io-html-report-layout-contract.md`
- optional template-specific layout contract from `templates/html/layouts/`

## Rendering rules

1. Use the universal cover, executive take, signal lens, structured detail rows, review section, source index and appendix structure.
2. Use domain-specific column labels from the layout contract.
3. Do not hardcode customer names; use values from IO Configuration, IO Run, Domain Context Profile and IO Output Artifact.
4. Use `IO Evidence Source.URL` for direct source links.
5. If a source URL is missing, create an internal link to the Evidence Index row.
6. Every material claim must have a source link or a `[REVIEW: source not found]` marker.
7. Needs Review findings must appear in a clearly labelled reviewer verification section.
8. Keep the generated HTML self-contained: inline CSS, no external scripts, no remote assets.
9. Save/render the output as an IO Output Artifact body or draft SharePoint file only after user/workflow confirmation.
10. Do not publish; leave the output in Review Required unless an approval process completes.

## Structured detail row contract

Each finding should render as:

| Signal Type | Topic / Signal | Evidence Summary | Relevance / Implication |
|---|---|---|---|

For competitor reports, use:

| Signal Type | Competitive Signal | Evidence Summary | Microsoft/Customer Relevance |

## Source link contract

For every finding row, add a source/reference link:

- direct external link if `IO Evidence Source.URL` exists;
- otherwise internal `#evidence-<evidence-id>` anchor.

Link text should be concise:

- `Read source`
- `Evidence details`
- `Review details`

## Output

Return or write:

- complete HTML string;
- source link count;
- findings without direct source URL;
- Needs Review count;
- recommended reviewer notes.
