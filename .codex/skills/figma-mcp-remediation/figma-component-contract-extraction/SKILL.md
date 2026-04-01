---
name: figma-component-contract-extraction
description: Supplemental enforcement pass for preflight contract quality. Use only when a preflight brief already exists and the representation, slot decisions, or parent property coverage still feel ambiguous.
---

# Component Contract Extraction

Load only after `figma-discovery-preflight` when the initial brief still has meaningful ambiguity.

## Goal

Pressure-test the existing preflight brief instead of producing a second full contract from scratch.

## Use This Skill When

- the brief has multiple plausible representation patterns
- slot vs property decisions are still shaky
- parent property coverage is incomplete
- reusable child vs local fragment decisions still feel unsafe

## Output

Return only the delta needed to unblock the write:

- `confirmedDecisions`
- `changedDecisions`
- `remainingAmbiguities`
- `askBeforeWrite`
- `knownGaps`

## Hard Gates

- Do not repeat the full preflight output if the first brief was already sufficient.
- Do not create a second discovery narrative.
- If ambiguity remains material, set `askBeforeWrite` and stop before writing.
