# Issue to Skill Map

This map links common issues to the smallest useful remediation layer.

| Reported issue | Skill(s) |
|---|---|
| Read-only discovery before any write | `figma-discovery-preflight` |
| Existing pages, QA pages, and component conventions must be inspected first | `figma-discovery-preflight` |
| Code token names do not match Figma token names | `figma-token-reconciliation` |
| A token substitute may be semantically unsafe | `figma-token-reconciliation` |
| Light/dark token parity must be verified | `figma-token-reconciliation`, `figma-variant-grid-qa`, `figma-dark-mode-semantic-qa` |
| Need a deterministic code-to-Figma contract before the first write | `figma-discovery-preflight` |
| Preflight exists but representation or parent-property decisions still feel ambiguous | `figma-component-contract-extraction` |
| Need to decide between variants, properties, and documented examples | `figma-discovery-preflight`, `figma-slot-properties-modeling`, `figma-variant-grid-qa` |
| Need to choose between a fixed-position docs shell and an auto-layout shell | `figma-discovery-preflight`, `figma-variant-grid-qa` |
| Variable binding or property wiring causes Figma API failures | `figma-api-safe-patterns` |
| Components are shape-only or missing text/effect styles | `figma-token-style-bindings` |
| Icon/text behavior should be modeled with component properties | `figma-slot-properties-modeling` |
| Reusable icon or spinner slots were flattened into loose vectors instead of child components | `figma-slot-properties-modeling`, `figma-discovery-preflight`, `figma-component-library-remediation` |
| A numeric scale token looked semantically right but resolved to the wrong value | `figma-token-reconciliation` |
| Existing pages or component sets should be reused, not duplicated | `figma-component-upsert` |
| Variant layout or QA presentation is unclear or overlapping | `figma-variant-grid-qa` |
| Documentation or hybrid presentation feels unfinished because content is loose on the page or lacks a real shell/background | `figma-variant-grid-qa` |
| Dark-mode live instances fail readability even though detached fallback proof looks acceptable | `figma-dark-mode-semantic-qa`, `figma-variant-grid-qa`, `figma-component-library-remediation` |
| The workflow stops with required work still unfinished | `figma-single-pass-complete` |

## Rule-Owned Guardrails

Rules should prevent these by default:

- Do not write before discovery is complete.
- Do not start writing before the representation pattern is selected.
- Do not start writing before the Figma-facing component contract is summarized.
- Do not start writing before the documentation shell layout model is selected when labeled grids or aligned sections are involved.
- Do not bind tokens before classifying the mapping.
- Do not assume property reference keys from labels.
- Do not silently replace semantic shadow/effect tokens with ad hoc local effects.
- Do not finish after structural QA alone when semantic mode parity is still unverified.
- Do not finish when only fallback QA artifacts pass but live-instance QA still fails in a required mode.
- Do not treat loose page content as completed documentation when a presentation shell/frame is expected.
- Do not flatten reusable slot-backed visuals into loose vectors when the code contract implies a child component.

## Default Vs Escalation

Default implementation overlays:

- `figma-discovery-preflight`
- `figma-token-reconciliation`
- `figma-token-style-bindings`
- `figma-api-safe-patterns`
- `figma-variant-grid-qa`

Escalation-only overlays:

- `figma-component-contract-extraction`
- `figma-component-upsert`
- `figma-slot-properties-modeling`
- `figma-dark-mode-semantic-qa`
- `figma-single-pass-complete`

## Subagent-Owned Audits

Subagents are best for:

- code contract discovery across the codebase
- Figma preflight across the target file and related component families
- approval-stage execution planning
- semantic QA against recipe intent and token behavior
- dark-mode semantic QA on live instances and fallback artifacts
- identifying which nested visuals are true child components versus local layout fragments
