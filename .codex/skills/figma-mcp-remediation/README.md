# Figma MCP Remediation Skills

This folder contains overlay skills that harden Figma component-library workflows without replacing the upstream Figma MCP skills.

Use upstream core skills for baseline behavior:

- `figma-use`
- `figma-generate-library`
- `figma-generate-design`
- `figma-implement-design`

Add remediation skills from this folder when the task needs stricter preflight, safer writes, or stronger validation.

## Architecture

The remediation system uses three layers:

- `Rules`
  - Always-on guardrails for preflight, token classification, API safety, and completion.
- `Skills`
  - Reusable workflows for discovery, reconciliation, API-safe writes, upsert, slot modeling, and QA.
- `Subagents`
  - Synthesis-heavy passes for discovery, contract extraction, and semantic QA.

## Skills In This Pack

- `figma-component-library-remediation`
  - Entry skill for the full remediation workflow.
- `figma-discovery-preflight`
  - Required read-only preflight before any write.
- `figma-component-contract-extraction`
  - Converts code and examples into a Figma-facing contract before the first write.
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

- `Discovery brief`
  - Synthesizes code, Figma conventions, token coverage, and risks.
- `Component contract extraction`
  - Derives the intended Figma-facing API from code and examples.
- `Semantic QA`
  - Reviews token correctness, mode parity, and recipe drift.
- `Dark-mode semantic QA`
  - Reviews live-instance readability, mode propagation, and fallback-vs-primary QA evidence.

Reusable prompt specs for these subagents live in:

- `subagents/discovery-brief.md`
- `subagents/component-contract-extraction.md`
- `subagents/semantic-qa.md`

## Scope Notes

- These skills target recurrent Figma MCP component-library failures.
- They stay composable: load only what is needed, or load the full overlay pack for stricter workflows.
