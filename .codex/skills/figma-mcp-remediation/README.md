# Figma MCP Remediation Skills

This folder contains overlay skills that harden Figma component-library workflows without replacing the upstream Figma MCP skills, the project rules, or the default `.cursor` subagent chain.

Use upstream core skills for baseline behavior:

- `figma-use`
- `figma-generate-library`
- `figma-generate-design`
- `figma-implement-design`

Add remediation skills from this folder when the task needs stricter implementation procedure, safer writes, or stronger validation.

## Architecture

The remediation system uses three layers:

- `Rules`
  - The single source of durable policy and hard gates.
- `Skills`
  - Implementation overlays that add procedure, output shapes, and targeted remediation.
- `Subagents`
  - The default orchestration path for code-backed component work in this repo.

## Ownership Model

Keep one owner per concern:

- Rules own policy.
- `.cursor` subagents own stage order and approval gates.
- Skills own implementation procedure and escalation.
- Reminder rules and checklist notes should point back to canonical rules instead of restating them.

## Default Workflow

The default repo workflow is the approval-gated chain defined by `figma-subagent-orchestration`:

1. `figma-code-contract`
2. `figma-preflight`
3. `figma-execution-planner`
4. human approval
5. `figma-implementation`
6. `component-validator`

This remediation pack is not the primary orchestration owner for that chain. Use it to strengthen implementation work inside or alongside the approved flow.

## Default vs Escalation Skills

Use these by default during implementation when relevant:

- `figma-discovery-preflight`
- `figma-token-reconciliation`
- `figma-token-style-bindings`
- `figma-api-safe-patterns`
- `figma-variant-grid-qa`

Treat these as escalation-only overlays:

- `figma-component-contract-extraction`
- `figma-component-upsert`
- `figma-slot-properties-modeling`
- `figma-dark-mode-semantic-qa`
- `figma-single-pass-complete`

## Skills In This Pack

- `figma-component-library-remediation`
  - Optional implementation router for this remediation pack. Do not treat it as the repo's top-level orchestration owner.
- `figma-discovery-preflight`
  - Required read-only preflight before any write.
- `figma-component-contract-extraction`
  - Escalation-only delta pass when the preflight brief still leaves representation or property decisions ambiguous.
- `figma-token-reconciliation`
  - Maps code semantics to Figma tokens and classifies each mapping.
- `figma-api-safe-patterns`
  - Safe Figma Plugin API patterns for bindings, properties, reruns, and recovery.
- `figma-component-upsert`
  - Idempotent page/component reuse and naming discipline.
- `figma-token-style-bindings`
  - Enforces token bindings, text styles, effect-style parity, and integer px output.
- `figma-slot-properties-modeling`
  - Enforces boolean, text, and instance-swap property modeling.
- `figma-variant-grid-qa`
  - Enforces readable documentation layout and separated QA pages.
- `figma-dark-mode-semantic-qa`
  - Validates dark-mode readability and semantic correctness on live component instances.
- `figma-single-pass-complete`
  - Prevents stopping with unfinished required work.

## Rule Responsibilities

Project rules should cover these universal behaviors:

- Do not write to Figma before discovery preflight is complete.
- Default component canvas work to the `Figma MCP Testing` file (`nPIzAMiRIezs04KU8kxrTF`) unless the user explicitly overrides it.
- Extract the component contract before deciding variants or properties.
- Select the correct Figma representation pattern before the first write.
- Select the documentation shell layout model before the first write when labels, headers, or mixed sections are involved.
- Classify each required token as `exact`, `substitute`, `missing`, or `unsafe` before binding.
- Do not assume component-property reference keys from display names.
- Require explicit effect-style parity handling when code semantics depend on reusable shadows or effects.
- Require mode-aware QA when multi-mode color collections are involved.
- Require live-instance QA proof in each required mode, not only detached fallback artifacts.
- Keep QA frames on testing pages and append instead of replacing.

## Subagent Responsibilities

Use subagents when the work is broader than a single skill execution:

- `Code contract`
  - Derives the code-backed component contract without touching Figma.
- `Preflight`
  - Synthesizes Figma conventions, component reuse, token coverage, and risks before any write.
- `Execution planning`
  - Produces the approval-stage plan and machine-readable implementation handoff.
- `Implementation`
  - Owns the write path only.
- `Component validation`
  - Reviews structure, composition, documentation, and QA completeness after writes.

Reusable prompt specs for these subagents live in:

- `subagents/discovery-brief.md`
- `subagents/component-contract-extraction.md`
- `subagents/semantic-qa.md`

## Scope Notes

- These skills target recurrent Figma MCP component-library failures.
- They stay composable: load only what is needed, or load the full overlay pack for stricter implementation work.
- If a skill and a rule overlap, prefer the rule for policy and keep the skill focused on procedure.
