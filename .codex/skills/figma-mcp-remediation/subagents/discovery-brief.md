# Discovery Brief Subagent

## Purpose

Use this subagent before any Figma component-library write when the task requires broad discovery across both code and the target Figma file.

## Recommended Subagent

- `subagent_type`: `explore`
- `model`: `fast`
- `readonly`: `true`

## Inputs To Provide

- target component name
- code paths to inspect:
  - recipe/theme file
  - component implementation
  - stories/examples
- target Figma file key or URL
- any known existing component page names

## Required Work

1. Inspect the code sources of truth.
2. Inspect the Figma file structure and identify:
   - pages
   - component authoring pages
   - testing/QA pages
   - existing component sets
   - variable collections
   - text styles
3. Check whether the target component already exists.
4. Identify candidate reference components already in the file.
5. Produce a risk list for likely token, layout, or API issues before writing.

## Required Return Format

Return a compact brief with:

- `sourceOfTruthFiles`
- `existingPages`
- `existingComponentPages`
- `existingTargetComponent`
- `testingPages`
- `variableCollections`
- `textStyles`
- `candidateReferenceComponents`
- `likelyContract`
- `atomicChildComponents`
- `parentCompositionPlan`
- `previewSurfaceStrategy`
- `pagePlacementPlan`
- `knownRisks`

## Prompt Template

```text
Create a discovery brief for building or updating the <COMPONENT> component in Figma.

Inspect these code sources:
- <RECIPE_PATH>
- <COMPONENT_PATH>
- <STORY_PATHS>

Inspect the target Figma file:
- <FIGMA_FILE_KEY_OR_URL>

Return only:
- sourceOfTruthFiles
- existingPages
- existingComponentPages
- existingTargetComponent
- testingPages
- variableCollections
- textStyles
- candidateReferenceComponents
- likelyContract
- atomicChildComponents
- parentCompositionPlan
- previewSurfaceStrategy
- pagePlacementPlan
- knownRisks

Do not propose writes. This is a read-only discovery pass.
```
