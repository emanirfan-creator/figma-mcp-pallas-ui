---
name: figma-token-style-bindings
description: Ensures components are bound to variables/tokens and typography uses text styles. Use to fix shape-only output, missing text styles, and rem->px decimal pixel issues.
---

# Token, Text Style, and Effect Style Bindings

## When to use
- The agent is creating components as raw shapes without binding fills/strokes/radii/spacing to variables.
- Text styling is missing (no text styles created or `textStyleId` not applied).
- Your rem->px conversion produces decimal pixel values in Figma sizing tokens.
- Variable scope/bindings are incomplete (colors/spacing/radii/text color not token-bound).

Load this skill with `figma-use` and `figma-generate-library`.

## Goal

Part 2 of the token pipeline: apply the approved mappings so all component outputs are design-system driven instead of shape-driven:

- Visual properties are bound to variables/tokens
- Typography uses text styles
- Effects use reusable effect styles when the code semantics require them
- Pixel values are integer-only where required

## Mandatory Rules

1. No unbound "shape-only" component output:
   - Components must bind fills/strokes/radius/spacing to variables when available.
2. Text style creation is required:
   - Create or reuse named text styles before assigning text visuals.
   - Apply `textStyleId` instead of only local hardcoded font properties.
3. Integer pixel normalization:
   - When converting rem to px, round to nearest integer before writing to Figma.
   - Never write decimal px values for size tokens.
4. Prefer aliasing semantic variables to primitive variables.
5. Explicitly define where tokens live in code:
   - Add mapping notes in return payload (e.g. `tokenSourceFiles`).
6. Effect style parity is required when code semantics depend on shadows or other reusable effects:
   - Reuse an existing local effect style when a semantically correct match exists.
   - If no correct local effect style exists, create one with deterministic naming and report that decision.
   - Do not silently replace a semantic shadow token with an untracked local effect blob.

## Binding Coverage Checklist

- Fill color -> variable-bound paint
- Stroke color -> variable-bound paint
- Frame gap/padding -> bound spacing variables
- Corner radius -> bound radius variables
- Text color -> text color variable
- Typography -> text style
- Shadow/effect token -> effect style or explicit reported fallback

## Effect Style Workflow

When the code uses semantic shadows or reusable effects:

1. Inspect local effect styles first.
2. If a semantically correct match exists, reuse it.
3. If the file has no matching effect style but the code token is clear and reusable, create a local effect style with stable naming.
4. If the correct effect depends on design preference rather than evidence, use `AskQuestion`.
5. Return the effect-style decisions alongside token mapping notes.

Recommended return fields when relevant:

- `effectStyleMappings`
- `effectStylesCreated`
- `effectStylesReused`
- `tokenSourceFiles`

## Failure Behavior

If no variable system is found:

- Stop and report missing token collections
- Ask for token source location or create a scoped minimum token set first
- Do not finish by committing hardcoded placeholders as final output
- Do not redo token classification here; use the mapping table from `figma-token-reconciliation`.
