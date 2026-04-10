# Code To Figma Component Workflow

This document describes the current default workflow for bringing a code-backed component into the Figma canvas in this project.

It is the human-readable companion to the project rules in `.cursor/rules/`. If this document and a canonical rule ever disagree, follow the rule and then update this file.

## Goal

Create or update a Figma component so it matches the code contract, uses reusable child components correctly, follows token and property rules, uses consistent documentation styling, and passes authoring and QA checks before completion.

## Workflow Modes

Choose the lightest workflow that still preserves safe modeling and QA.

### Full Workflow

Use the full approval-gated six-step workflow when:

- creating a new component
- changing composition model
- introducing or reworking reusable child components
- token reconciliation is nontrivial
- dark mode or the full QA matrix materially matters

Sequence:

1. `figma-code-contract`
   Code-only discovery and contract extraction.
2. `figma-preflight`
   Read-only Figma-context evaluation. This stage must also read `docs/pallas-ui-component-mapping.md`.
3. `figma-execution-planner`
   Produces:
   - a short human-readable execution plan for approval
   - a machine-readable implementation handoff
4. human approval
   Required gate between planning and any write work.
5. `figma-implementation`
   Executes the approved plan and returns an implementation report.
6. `component-validator`
   Report-only QA. If QA fails, implementation must run again before completion.

### Fast Workflow

Use the fast workflow when all of the following are true:

- updating an existing component
- the code contract is already obvious
- no slot or property ambiguity exists
- no new child reuse decisions are needed

Preferred fast sequence:

1. `figma-code-contract`
   Keep the code contract explicit so the write path still has a clear source of truth.
2. `figma-preflight`
   Confirm the existing component, Figma context, and whether the work still qualifies as fast.
3. inline plan or compact `figma-execution-planner` handoff
   Use the lightest planning form that keeps the write path unambiguous.
4. `figma-implementation`
   Reuse upstream discovery instead of rediscovering the same Figma state.
5. targeted QA
   Use `component-validator` when the implementation reports risk, token substitution, dark-mode uncertainty, or QA ambiguity.

If fast-workflow eligibility breaks at any point, escalate back to the full workflow.

## Canonical Rules

These always-applied rules own the policy:

- `figma-preflight-required`
- `figma-content-slot-detection`
- `figma-slot-composition-hard-gate`
- `figma-token-classification`
- `figma-api-safety`
- `figma-component-set-documentation`
- `figma-qa-completion`
- `figma-target-file`
- `figma-subagent-orchestration`

## Optional Reminder Rules

These notes can help retrieval or focused audits, but they are not policy owners:

- `figma-design-system-reviewer-checklist`

The thinner reminder stubs for live-mode QA, usage-first modeling, atomic composition, code-first child audit, parent property coverage, and authoring-page validation have been intentionally collapsed into this workflow doc plus the canonical rules above. Prefer this document as the lightweight index instead of maintaining separate pointer files.

## Stage Responsibilities

### 1. Code Evaluator

`figma-code-contract` inspects the code source of truth:
- component API
- real usage
- stories or examples
- optional and required regions
- variant axes
- reusable child components
- token references

This stage is code-only and must not inspect Figma.

### 2. Figma Preflight

`figma-preflight` consumes the code-contract handoff and inspects:
- existing pages and component families
- testing pages
- variable collections
- text styles
- target component existence

This stage must also read `docs/pallas-ui-component-mapping.md` and use it as Pallas-specific modeling guidance.

Preflight also owns the canonical Figma inventory snapshot for the current run:
- existing pages
- existing component pages and candidate families
- testing pages
- variable collections
- text styles

Downstream stages should reuse that inventory unless the implementation detects it is stale or contradicted by current file state.

### 3. Approval Planning

`figma-execution-planner` consumes the code and preflight handoffs and produces:
- a short human-readable execution plan for review
- a machine-readable implementation handoff for downstream execution

No write work should happen before this plan is reviewed and approved.

### 4. Human Approval

`figma-execution-planner` must stop before writes.

- review the execution plan
- approve or revise the plan
- only then continue to implementation

### 5. Implementation

`figma-implementation` executes the approved plan:
- child-first build order when needed
- token and text-style reconciliation
- safe upsert
- correct composition
- documentation shell and authoring-page layout

Implementation owns the write path, but not final QA signoff.

### 6. QA

`component-validator` is the report-only QA stage:
- validate structure
- validate composition
- validate slot/property correctness
- validate documentation quality
- validate authoring-page quality
- validate QA completeness

If QA fails, the work must return to implementation for another pass.

## Ownership Model

Keep one owner per concern:

- Rules own durable policy and hard gates.
- The chained subagent workflow owns stage order and approval boundaries.
- Skills own implementation procedure, output shapes, and targeted remediation.
- Reminder rules and checklist rules should point back to canonical rules instead of restating them.

## Implementation Support Skills

These remain implementation support tools rather than top-level workflow stages.

Default implementation overlays:

- `figma-use`
- `figma-token-reconciliation`
- `figma-token-style-bindings`
- `figma-api-safe-patterns`
- `figma-variant-grid-qa`

Optional router:

- `figma-component-library-remediation`
  Use when you want a single implementation-phase overlay router. It should not replace the default `.cursor` subagent chain.

Escalation-only overlays:

- `figma-component-upsert`
- `figma-slot-properties-modeling`
- `figma-component-contract-extraction`
- `figma-dark-mode-semantic-qa`
- `figma-single-pass-complete`

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

Default to the full workflow unless the request clearly qualifies for the fast workflow.

- `figma-code-contract`
- `figma-preflight`
- `figma-execution-planner`
- human approval
- `figma-implementation`
- `component-validator`

Use the fast workflow only when all of the following are true:

- updating an existing component
- the code contract is already obvious
- no slot or property ambiguity exists
- no new child reuse decisions are needed

Escalate to the full workflow when any of the following are true:

- creating a new component
- changing composition model
- introducing or reworking reusable child components
- token reconciliation is nontrivial
- dark mode or the full QA matrix materially matters

Do not add extra discovery or implementation subagents by default. Use `explore` or `generalPurpose` only when a blocker clearly requires broader investigation, or when the user explicitly asks for a different execution model.

## Inventory Reuse

Avoid rediscovering the same Figma file context at every stage.

- `figma-preflight` should capture the current run's Figma inventory snapshot.
- `figma-execution-planner`, `figma-implementation`, and `component-validator` should consume that snapshot instead of re-listing pages, candidate families, variable collections, and text styles from scratch.
- Refresh the inventory only when a downstream stage detects staleness, missing context, or contradictory file state.

## Completion Criteria

Do not call a component complete until all of the following are true:

- code evaluation is complete
- preflight is complete
- planner approval has happened
- region classification is resolved
- reusable child composition is correct
- token and text-style mappings are safe
- documentation shell matches the canonical visual system
- authoring-page layout is readable and non-overlapping
- testing-page QA exists for required modes
- report-only QA passes for structure, composition, properties, documentation, and QA

## Short Operational Summary

The default full sequence is:

`figma-code-contract -> figma-preflight -> figma-execution-planner -> human approval -> figma-implementation -> component-validator -> implementation if QA fails`

The preferred fast sequence is:

`figma-code-contract -> figma-preflight -> compact planning -> figma-implementation -> targeted QA when risk warrants it`
