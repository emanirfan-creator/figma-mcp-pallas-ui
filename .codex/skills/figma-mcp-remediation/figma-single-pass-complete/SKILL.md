---
name: figma-single-pass-complete
description: Short completion discipline appendix for Figma workflows. Use only when an agent is at risk of stopping before the approved scope and required quality gates are actually complete.
---

# Single-Pass Completion

## When to use
- The agent is at risk of stopping after partial progress.
- The router or main workflow already exists, but completion discipline needs reinforcement.

Load with `figma-use` and whichever workflow skill applies (`figma-generate-library` or `figma-generate-design`).

## Goal

Complete the approved scope in one continuous run plan instead of stopping early with "I can do next pass".

## Mandatory Rules

1. Convert the request into an explicit completion checklist before first write call.
2. Execute tasks sequentially and finish all checklist items unless blocked by:
   - missing permissions
   - missing assets
   - explicit user stop
3. After each write step, validate and continue immediately to next checklist item.
4. Do not end on optional follow-up suggestions when required checklist items remain.
5. If blocked, report:
   - exact blocker
   - current completed items
   - exact next command/script to resume

## Minimal Completion Template

Track and return:

- `plannedSteps`
- `completedSteps`
- `remainingSteps`
- `blockers`
- `resumeInputs` (IDs/keys needed for continuation)
- `authoringPageAcceptance`
  - `atomicChildReadable`
  - `componentSetBoundsValid`
  - `docsShellHugsContent`
  - `topLevelOverlapFree`

## Quality Gate Before Done

All must be true:

- No duplicate component/page assets created
- No overlap in generated layouts
- Tokens/styles/properties bound as required
- Authoring-page acceptance passes:
  - atomic child documentation is readable
  - component-set bounds cover the full laid-out content
  - documentation shell hugs its content with the intended padding
  - top-level authoring artifacts do not overlap
- QA artifacts created on light/dark testing pages
- Live-instance QA is present in every required mode, or any unresolved failure is explicitly reported as a blocker with preserved fallback evidence
- All requested components and variants are visible and review-ready
- Prefer the main router skill as the default owner of this checklist.
