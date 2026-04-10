---
name: figma-component-library-remediation
description: Orchestrates the remediation overlay pack for any component-agnostic Figma library workflow. Use to apply hard quality gates for upsert, token/text bindings, slot properties, variant grid/QA placement, and single-pass completion.
---

# Component Library Remediation Router

This is the entry skill for the remediation pack.

Use it as an implementation-phase router, not as the repo's top-level orchestration owner. The default stage order and approval gates live in the `.cursor` subagent workflow and the always-applied rules.

## When to use
- You are building or updating a Figma design system component library and want all remediation gates enabled.
- You previously saw one or more failures: shape-only components, missing text styles, duplicated components on reruns, overlapping variant grids, or QA/test placement issues.
- You want the workflow to finish the full approved scope before stopping.
- A preflight brief or approved implementation handoff already exists and you want a tighter implementation overlay.

## Router

Default all canvas writes to the `Figma MCP Testing` file (`nPIzAMiRIezs04KU8kxrTF`) unless the user explicitly chooses another file.

Keep policy ownership clear:

- Rules own durable policy and hard gates.
- This skill owns only implementation routing inside the approved workflow.

Load the smallest valid chain for the task:

### 1. Full component build or major update
1. `figma-use`
2. `figma-generate-library`
3. `figma-discovery-preflight`
4. `figma-token-reconciliation` when tokenized
5. `figma-api-safe-patterns`
6. `figma-component-upsert`
7. `figma-token-style-bindings` when tokenized
8. `figma-slot-properties-modeling` only if slots/properties/composition still need write-phase enforcement
9. `figma-variant-grid-qa`
10. `figma-dark-mode-semantic-qa` only if dark-mode semantics remain in doubt
11. `figma-single-pass-complete` only if completion discipline is at risk

### 2. Token-only remediation
1. `figma-use`
2. `figma-token-reconciliation`
3. `figma-api-safe-patterns`
4. `figma-token-style-bindings`
5. `figma-variant-grid-qa` when QA proof must be refreshed

### 3. Layout, docs, or QA remediation
1. `figma-use`
2. `figma-component-upsert` when updating an existing family
3. `figma-variant-grid-qa`
4. `figma-dark-mode-semantic-qa` only when mode semantics are still failing

### 4. Upsert or rerun duplication remediation
1. `figma-use`
2. `figma-api-safe-patterns`
3. `figma-component-upsert`

## Subagent Guidance

- Prefer direct tools for narrow file lookups, known paths, or single-symbol questions.
- Use one discovery path per component: parent-led preflight or one delegated discovery brief, not both.
- Use `explore` only for broad unknowns across code and Figma structure.
- Use `generalPurpose` only for synthesis-heavy structured outputs.
- Use `shell` only for command execution.
- Do not stack a remediation skill with an equivalent-role subagent unless responsibilities are clearly divided.
- Do not replace the default `figma-code-contract -> figma-preflight -> figma-execution-planner -> human approval -> figma-implementation -> component-validator` chain with this router.

## Hard Gates

- Do not write before one explicit preflight brief exists for build-scale work.
- Do not write if any candidate region is unclassified or any material ambiguity remains un-escalated.
- Do not bind tokens before classification.
- Do not complete the work until authoring-page checks, testing-page QA, and review gates pass.
