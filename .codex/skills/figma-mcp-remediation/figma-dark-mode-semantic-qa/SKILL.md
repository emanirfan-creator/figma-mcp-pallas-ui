---
name: figma-dark-mode-semantic-qa
description: Focused dark-mode investigation pass for tokenized Figma components. Use only when the main authoring-and-QA pass still leaves dark-mode semantics or readability in doubt.
---

# Dark Mode Semantic QA

Load after the main authoring-and-QA pass and before marking the work complete.

## Goal

Confirm that dark-mode output is semantically correct and readable on live component instances, not only on detached or manually restyled fallback examples.

## Required Checks

Validate all of the following:

- at least one live component instance screenshot exists for dark mode
- text remains readable against the resolved dark surfaces
- placeholder, secondary, tertiary, and disabled text treatments are still distinguishable and usable
- slot-backed visuals such as icons, spinners, and badges remain readable in dark mode
- dark-mode behavior matches the intended component role, not just a visually acceptable override

## Workflow

1. Inspect the dark-mode QA frame built from live instances.
2. If readability looks wrong, investigate in this order:
   - variable mode application on the QA frame
   - variable mode application on live instances or relevant descendants
   - token mapping quality from the reconciliation step
   - text and paint bindings on the affected nodes
3. Re-screenshot the live dark-mode surface after each targeted fix.
4. Only add a fallback QA artifact when:
   - the live-instance issue is still unresolved, and
   - keeping reviewable proof is better than stopping without dark-mode evidence
5. If a fallback artifact is added:
   - preserve the failing live-instance QA proof
   - append the fallback as a separate QA frame
   - label the fallback clearly as non-primary evidence
   - report the remaining live-instance blocker explicitly

## Output

Return:

- `liveDarkQaNodeIds`
- `issuesFound`
- `fixesAttempted`
- `fallbackArtifacts`
- `remainingBlockers`
- `completionDecision`

## Completion Decision Rules

- `pass`:
  - live dark-mode instances are readable and semantically correct
- `pass_with_blocker_reported`:
  - fallback proof exists, but the unresolved live-instance issue is explicitly documented
- `fail`:
  - no usable live dark-mode proof exists, or semantic readability is still unclear

## Hard Gates

- Do not treat detached or manually restyled dark examples as a replacement for live-instance QA.
- Do not call dark-mode QA complete if the only passing proof is fallback content and the unresolved live-instance issue is not explicitly reported.
- Do not silently compensate for collapsed semantic text tiers with arbitrary opacity changes; either justify them through token reconciliation or report the gap.
