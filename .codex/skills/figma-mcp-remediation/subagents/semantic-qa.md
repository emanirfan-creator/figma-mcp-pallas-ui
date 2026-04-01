# Semantic QA Subagent

## Purpose

Use this subagent after a Figma component build when structural validation passed but you still need an independent audit for semantic token correctness, recipe drift, and mode parity.

## Recommended Subagent

- `subagent_type`: `generalPurpose`
- `model`: `fast`
- `readonly`: `true`

## Inputs To Provide

- target component name
- recipe/theme file path
- component implementation path when relevant
- token reconciliation output
- Figma metadata for the authoring page or component set
- screenshots for:
  - authoring page
  - light QA frame
  - dark QA frame
  - any live-instance evidence distinct from detached or fallback examples when mode QA matters

## Required Work

1. Compare the built Figma artifact against the intended recipe/contract.
2. Check whether token mappings are semantically correct, not just present.
3. Check light/dark mode parity.
4. Distinguish live-instance QA evidence from detached, fallback, or manually restyled evidence.
5. Fail or flag the audit when required mode proof exists only as detached or fallback evidence.
6. Flag likely recipe drift, unsafe substitutions, or missing QA coverage.
7. If the final judgment depends on user preference or design intent rather than clear evidence, recommend that the parent agent use `AskQuestion`.

## Required Return Format

Return:

- `passFail`
- `findings`
- `tokenConcerns`
- `modeParityIssues`
- `liveInstanceCoverage`
- `recipeDrift`
- `recommendedFixes`
- `questionsForUser`

## Prompt Template

```text
Perform a semantic QA audit for the <COMPONENT> Figma component.

Inspect:
- <RECIPE_PATH>
- <COMPONENT_PATH_OPTIONAL>
- <TOKEN_RECONCILIATION_OUTPUT>
- <FIGMA_METADATA>
- <AUTHORING_SCREENSHOT>
- <LIGHT_QA_SCREENSHOT>
- <DARK_QA_SCREENSHOT>

Return only:
- passFail
- findings
- tokenConcerns
- modeParityIssues
- liveInstanceCoverage
- recipeDrift
- recommendedFixes
- questionsForUser

Focus on semantic correctness, token behavior, and mode parity rather than just layout overlap.
```
