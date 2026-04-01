---
name: figma-discovery-preflight
description: Produces the canonical read-only preflight and Figma-facing contract before any component-library write. Use when building or updating a code-backed component in Figma and you need one compact brief covering discovery, modeling, composition, documentation, and QA scope.
---

# Discovery Preflight And Contract

Load with `figma-use` and, when relevant, `figma-generate-library`.

## Goal

Produce one compact preflight brief before the first write so the build uses the file's real conventions and does not invent structure mid-run.

## Workflow

1. Inspect the code source of truth:
   - component implementation
   - recipe/theme definition
   - stories/examples/docs
2. Inspect the Figma file:
   - pages
   - existing component pages and similar families
   - testing/QA pages
   - variable collections and text styles when tokenized
3. Check whether the target component already exists.
4. Extract the contract before any write:
   - component purpose and intended reuse
   - smallest valid real-world instance
   - realistic usage combinations
   - stable shell
   - true variant axes
   - boolean, text, and instance-swap properties
   - candidate region classifications
   - reusable child components and local fragments
   - whether designers start from the molecule or from atoms
   - whether repeated child structure needs guided composition
   - documentation shell choice
   - QA scope and required modes
5. Return one preflight brief before any write call.

## Required Output

Return:

- `componentPurpose`
- `usageIntent`
- `realWorldUsageMatrix`
- `sourceOfTruthFiles`
- `existingPages`
- `existingComponentPages`
- `existingTargetComponent`
- `testingPages`
- `variableCollections`
- `textStyles`
- `candidateReferenceComponents`
- `representationPattern`
- `representationAlternatives`
- `stableShell`
- `variantAxes`
- `properties`
- `regionClassification`
- `areaDecisionTable`
- `atomicChildComponents`
- `localFragments`
- `parentCompositionPlan`
- `parentPropertyCoverage`
- `documentationPlan`
- `qaPlan`
- `ambiguitiesRequiringHumanDecision`
- `askBeforeWrite`
- `writeBlockedUntil`
- `knownRisks`

## Hard Gates

- Do not create or update nodes during preflight.
- Do not start writing if the target component already exists and has not been inspected.
- Do not skip QA page discovery.
- Do not skip variable/text-style discovery when the component is tokenized.
- Do not reduce the preflight to a visual recap.
- Do not assume the fullest example is the default component contract.
- Do not leave `ambiguitiesRequiringHumanDecision` implicit.
- Do not leave any candidate region unclassified.
- If a reusable visual child is ambiguous, stop and ask before writing.
- If the right representation remains unclear, set `askBeforeWrite` and use `AskQuestion`.

## Notes

- This skill replaces the old split between discovery preflight and contract extraction.
- Use `figma-slot-properties-modeling` only when the write phase needs extra enforcement help after this brief exists.

## Compact Output Shape

```yaml
componentPurpose: Stable job the component performs.
usageIntent:
  - Primary navigation control reused across product surfaces
realWorldUsageMatrix:
  - trigger only
  - trigger + label
representationPattern: Parent component with atomic child trigger component
representationAlternatives:
  - atoms-only composition
stableShell: Tabs container stays stable; label and icon are property-driven per trigger
variantAxes:
  - selected
properties:
  boolean:
    - showIcon
  text:
    - label
  instance-swap:
    - icon
regionClassification:
  icon: atomic-child-component
  label: property-driven-region
  indicator: fixed-shell-structure
areaDecisionTable:
  - "| Area | Decision | Reason |"
atomicChildComponents:
  - TabTrigger
localFragments: []
parentPropertyCoverage:
  label:
    boolean: Show Label
    text: Label
documentationPlan:
  shellLayout: fixed-position shell
  previewSurfaceStrategy: Neutral surface for contrast-safe trigger previews
  pagePlacementMap:
    - docs shell
    - trigger section
    - parent component set
qaPlan:
  liveInstanceChecks:
    - selected vs unselected trigger
  requiredModes:
    - light
    - dark
ambiguitiesRequiringHumanDecision: []
askBeforeWrite: false
writeBlockedUntil: none
```
