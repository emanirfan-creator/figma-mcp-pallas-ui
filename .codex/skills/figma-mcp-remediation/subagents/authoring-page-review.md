# Authoring Page Review Subagent

## Purpose

Use this subagent after a Figma component build and before final completion to independently validate the authoring page as a review-ready surface.

## Recommended Subagent

- `subagent_type`: `generalPurpose`
- `model`: `fast`
- `readonly`: `true`

## Inputs To Provide

- target component name
- authoring page metadata
- component set metadata when separate
- authoring page screenshot
- screenshots for any atomic child documentation sections
- planned page placement map when available

## Required Work

1. Inspect the authoring page structure and screenshots.
2. Decide whether atomic child documentation remains readable on its preview surface.
3. Decide whether the component-set bounds cover the full laid-out content.
4. Decide whether the documentation shell hugs its documented content with the intended padding.
5. Report any top-level overlap or collision risk among authoring-page artifacts.
6. Return blocking issues instead of soft suggestions when any authoring-page acceptance gate fails.

## Required Return Format

Return:

- `atomicChildReadable`
- `componentSetBoundsValid`
- `docsShellHugsContent`
- `topLevelOverlapReport`
- `blockingIssues`
- `recommendedFixes`

## Prompt Template

```text
Review the authoring page for the <COMPONENT> Figma component.

Inspect:
- <AUTHORING_PAGE_METADATA>
- <COMPONENT_SET_METADATA_OPTIONAL>
- <AUTHORING_PAGE_SCREENSHOT>
- <ATOMIC_CHILD_SCREENSHOTS_OPTIONAL>
- <PAGE_PLACEMENT_PLAN_OPTIONAL>

Return only:
- atomicChildReadable
- componentSetBoundsValid
- docsShellHugsContent
- topLevelOverlapReport
- blockingIssues
- recommendedFixes

Fail the review when authoring-page acceptance is not yet ready, even if testing-page QA already exists.
```
