# Code To Figma Component Workflow

This document describes the current default workflow for bringing a code-backed component into the Figma canvas in this project.

It is the human-readable companion to the project rules in `.cursor/rules/`. If this document and a canonical rule ever disagree, follow the rule and then update this file.

## Goal

Create or update a Figma component so it matches the code contract, uses reusable child components correctly, follows token and property rules, uses consistent documentation styling, and passes authoring and QA checks before completion.

## Default Skills

Load these skills as needed during the workflow:

- `figma-discovery-preflight`
  Use first for the required read-only preflight before any component-library write.
- `figma-use`
  Mandatory before Figma write actions that depend on the write workflow.
- `figma-component-library-remediation`
  Use for broader component-library quality enforcement.
- `figma-component-upsert`
  Use when creating or updating component sets without duplication.
- `figma-token-reconciliation`
  Use when code tokens need mapping to Figma variables.
- `figma-token-style-bindings`
  Use when token and text-style binding quality matters.
- `figma-api-safe-patterns`
  Use for safe property wiring, variable binding, and rerun-safe writes.
- `figma-slot-properties-modeling`
  Use when write-phase modeling risks flattening reusable children or misusing slots.
- `figma-component-contract-extraction`
  Use only if preflight exists but the component contract still feels ambiguous.
- `figma-variant-grid-qa`
  Use when documenting and validating laid-out variants and QA pages.

## Core Rules

These rules shape the workflow:

- `figma-preflight-required`
- `figma-content-slot-detection`
- `figma-slot-composition-hard-gate`
- `figma-token-classification`
- `figma-api-safety`
- `figma-component-set-documentation`
- `figma-qa-completion`
- `figma-design-system-reviewer-checklist`
- `figma-target-file`
- `figma-subagent-orchestration`

## Workflow Steps

1. Inspect the code source of truth.
   Read the component API, real usage, stories if present, variant axes, optional regions, reusable child components, and token references. Do not model from a screenshot alone.

2. Run the required read-only preflight.
   Produce a compact brief covering:
   - component purpose
   - usage intent
   - real-world usage matrix
   - representation pattern
   - stable shell
   - variant axes
   - properties
   - region classification
   - atomic child components
   - local fragments
   - documentation plan
   - QA plan
   - ambiguities requiring human decision
   - write blockers

3. Decide the representation before writing.
   Determine whether the target is:
   - a true atomic component
   - a molecule or parent assembled from reusable children
   - a property-driven component
   - a component with a true content slot

4. Audit reusable child composition.
   Check for code-backed children such as `Icon`, `Badge`, `Avatar`, `Spinner`, `Trigger`, or `Button`. Reuse matching children in Figma when the code contract requires them. Do not bake reusable children into loose local frames.

5. Confirm the target Figma file and existing assets.
   Default to the `Figma MCP Testing` file unless the user explicitly approves another target. Reuse existing component sets, pages, variants, variables, styles, and approved documentation shells when possible.

6. Reconcile tokens and text styles.
   Classify required tokens as `exact`, `substitute`, `missing`, or `unsafe`. Bind fills, strokes, effects, and text styles semantically rather than by loose visual similarity.

7. Build or repair atomic children first when needed.
   If a parent depends on reusable child components, create or upsert those children before composing the parent.

8. Upsert the target component safely.
   Reuse existing component sets and exact-match variants where possible. Add required parent properties before combining variants. Resolve actual Figma property keys after creation. Keep reruns deterministic.

9. Compose the parent correctly.
   Put child axes on children and parent axes on parents. Use booleans, text props, instance swaps, and variants at the correct level. Avoid loose vectors or baked copies when the code contract implies reuse.

10. Lay out and document the component set consistently.
   Documentation style is part of the contract, not a freeform design step.

## Documentation Standard

All component documentation shells should share the same visual system unless the user explicitly approves an exception.

- Use real `FrameNode`s and auto layout for final documentation structures.
- Do not use groups as final authoring output.
- Use shell fill token `Color/Surface/Elevated`.
- Use shell stroke token `color/Border`.
- Use shell stroke weight `1px`.
- Use shell radius token `radii/md`.
- Use Figma text styles by semantic role rather than local text formatting.
- Use spacing tokens consistently for shell padding, section gaps, row gaps, and item gaps.
- Keep one consistent spacing rhythm across all documentation shells.
- Keep atomic-child documentation in a separate top-level documentation frame.
- Keep molecule or parent documentation in a separate top-level documentation frame.
- Reflow `ComponentSetNode` children into a readable grid after `combineAsVariants`.
- Resize the `ComponentSetNode` to the furthest child bounds after reflow.

## Subagent Policy

Use subagents selectively to improve output quality. The main agent should still own orchestration and the actual write path.

- Small atomic component with obvious API and no composition risk:
  No subagent is required unless ambiguity remains.
- Molecule, parent component, or component with reusable children:
  Use a discovery or composition-audit subagent before writing.
- Tokenized, high-risk, or high-importance component:
  Use discovery plus validation coverage before completion.
- Broad library migration or repeated component-conversion work:
  Strongly prefer subagents for discovery and validation.

Recommended roles:

- `explore`
  Use for code discovery, usage mining, token references, and related-component searches.
- `generalPurpose`
  Use for deeper multi-file contract research when the evidence is spread across many files.
- `component-validator`
  Use for reviewer-style validation of structure, composition, documentation consistency, authoring quality, and QA completeness.

Do not split the actual Figma write across multiple subagents by default.

## Completion Criteria

Do not call a component complete until all of the following are true:

- preflight is complete
- region classification is resolved
- reusable child composition is correct
- token and text-style mappings are safe
- documentation shell matches the canonical visual system
- authoring-page layout is readable and non-overlapping
- testing-page QA exists for required modes
- reviewer-style validation passes for structure, composition, properties, documentation, and QA

## Short Operational Summary

The default sequence is:

`code audit -> preflight -> modeling -> child reuse audit -> token/style reconciliation -> safe upsert/build -> documentation shell -> QA -> reviewer pass`
