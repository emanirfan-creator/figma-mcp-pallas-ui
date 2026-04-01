# Component Contract Extraction Subagent

## Purpose

Use this subagent when the component API is not obvious from a single source and you need a Figma-ready contract extracted from code, recipes, and examples before writing.

## Recommended Subagent

- `subagent_type`: `generalPurpose`
- `model`: `fast`
- `readonly`: `true`

## Inputs To Provide

- target component name
- recipe/theme file path
- component implementation path
- story/example paths
- any discovery brief already produced

## Required Work

1. Inspect the recipe or theme definition.
2. Inspect the component implementation.
3. Inspect the stories/examples.
4. Resolve the intended Figma-facing API:
   - representation pattern
   - variant axes
   - default variants
   - text properties
   - boolean properties
   - instance-swap properties
   - content slots
   - atomic child components
   - property-driven regions
   - local fragments
   - fixed shell structure
   - slot decisions
   - wrong slot risks
   - documentation shell layout
   - QA scope
   - out-of-scope behavior for v1
5. Flag ambiguities that require user judgment.
6. If ambiguities remain material, recommend that the parent agent use `AskQuestion` with explicit choices instead of inferring the answer.

## Required Return Format

Return:

- `componentName`
- `representationPattern`
- `variantAxes`
- `defaultVariants`
- `textProperties`
- `booleanProperties`
- `instanceSwapProperties`
- `contentSlots`
- `atomicChildComponents`
- `propertyDrivenRegions`
- `localFragments`
- `fixedShellStructure`
- `slotDecisions`
- `wrongSlotRisks`
- `documentationShellLayout`
- `qaScope`
- `outOfScope`
- `ambiguities`

## Prompt Template

```text
Extract a Figma-ready contract for the <COMPONENT> component.

Inspect:
- <RECIPE_PATH>
- <COMPONENT_PATH>
- <STORY_PATHS>

If provided, use this discovery brief as context:
<DISCOVERY_BRIEF>

Return only:
- componentName
- representationPattern
- variantAxes
- defaultVariants
- textProperties
- booleanProperties
- instanceSwapProperties
- contentSlots
- atomicChildComponents
- propertyDrivenRegions
- localFragments
- fixedShellStructure
- slotDecisions
- wrongSlotRisks
- documentationShellLayout
- qaScope
- outOfScope
- ambiguities

This is a read-only contract extraction task.
```
