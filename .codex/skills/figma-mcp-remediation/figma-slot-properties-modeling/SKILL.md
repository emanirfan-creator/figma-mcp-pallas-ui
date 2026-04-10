---
name: figma-slot-properties-modeling
description: Supplemental write-phase enforcement for properties, slot decisions, and child-component wiring. Use only when the preflight brief exists but the write phase still risks duplicating behavior or flattening reusable children.
---

# Slot Properties Modeling

Load after `figma-discovery-preflight` when extra write-phase enforcement is needed.

This is an escalation-only overlay. Canonical modeling policy lives in `figma-content-slot-detection` and `figma-slot-composition-hard-gate`.

## When to use
- The agent makes icon/text variants by splitting them into separate component families instead of using boolean/text/instance-swap properties.
- A true content region such as modal body, content area, or footer is being flattened into fixed parent structure.
- Icon-only or mandatory-slot-hidden behavior is not reachable via component properties.
- Required text properties (e.g. labels) are missing or not wired via component properties.
- Reusable visual child regions are being represented as loose vectors or ad hoc frames instead of real child components.

## Goal

Model each nested region with the correct mechanism instead of overloading the word `slot`:

- Content-region slots for consumer-editable body areas inside stable shells
- Guided composition regions on parent molecules when approved nested children need to grow or shrink in normal use
- Boolean properties for visibility/state toggles
- Text properties for labels/content values
- Instance-swap properties for swappable child visuals or nested components
- Child component instances for reusable structural visuals before the parent is combined into variants
- Local fragments for one-off internal structure

For each customizable area, state the decision and a one-line reason before writing.

This skill supplements the preflight brief; it should not recreate a second full planning pass.

## Mandatory Rules

1. Do not model content-driven or property-driven behavior as separate component families.
2. Classify each candidate region as one of:
   - `content-slot`
   - `atomic-child-component`
   - `property-driven-region`
   - `local-fragment`
   - `fixed-shell-structure`
3. Use a `content-slot` only for a consumer-editable region inside a stable shell:
   - examples: modal body, drawer content area, card body, footer area when intended to vary by use case
   - require composition-level variation, not just simple value or visibility changes
   - require enough openness that strict property modeling would otherwise push designers toward detaching
4. Use a guided composition region on the parent when:
   - designers normally start from the full molecule rather than from atoms
   - repeated structured child content must be duplicated, removed, or reordered in common use
   - preferred nesting or equivalent structure can constrain what may be inserted
   - the region should stay system-safe rather than becoming a freeform content area
5. Use boolean property patterns for visibility-driven behavior:
   - `Show Text` boolean
   - `Show Icon` boolean
   - component-equivalent visibility toggles for optional subregions
6. Use text properties for scalar content:
   - `Label` as TEXT property
   - placeholder/title/helper text when the region does not need arbitrary composition
7. Use instance-swap properties for swappable child visuals or nested components:
   - `Icon` as INSTANCE_SWAP property
   - other nested child injections only when the contract is still property-driven rather than body-content driven
8. If the code accepts a reusable visual prop such as `icon`, `spinner`, `avatar`, `badge`, or similar, default it to child-component or instance-swap modeling before considering a `content-slot`.
9. Do not classify decorative chrome or implementation details as `content-slot` just because they are swappable in code:
   - icons
   - chevrons
   - close affordances
   - checkmarks
   - one-off adornments
10. Reuse existing child components where possible; avoid duplicating equivalent assets.
11. Do not finish with raw vectors directly inside the parent when those vectors stand in for a reusable child-component contract.
12. Add required component properties before `combineAsVariants`; if the parent has already been combined and the needed property model cannot be added safely, rebuild the component set instead of silently shipping a weaker structure.
13. When choosing between strict properties, guided parent composition, and flexible slots, always consider:
   - whether the component is a design-system base or a product-specific pattern
   - whether multiple teams will reuse it
   - whether item count or order can vary
   - whether designers would otherwise need to detach for common usage
   - whether designers usually begin with the molecule or the atom
   - whether preferred nesting can preserve structure while allowing repeated child growth
14. If a region is ambiguous between property and slot modeling, stop and ask exactly:
   - `Should [area name] be strict (property) or flexible (slot)?`
   - `For example: [give a concrete use case for each option]`
15. If the overall component is ambiguous between atoms-only composition and a guided parent molecule, stop and ask exactly:
   - `Should [component name] be atoms-first or a guided parent molecule?`
   - `For example: [show one designer workflow for each option]`
16. Do not proceed with building until every ambiguous region is resolved.

## Modeling Taxonomy

- Variant property -> true recipe/layout axis
- Boolean property -> conditional structure or visibility only
- Text property -> scalar content only
- Instance swap -> swappable child/component injection only
- Content slot -> consumer-editable body region inside a stable shell
- Guided composition region -> constrained editable region inside a parent molecule for approved repeated child structure
- Child component -> reusable structural contract that should not be flattened into the parent
- Local fragment -> one-off internal structure with no reusable contract

## Output Format

- End the modeling decision pass with a summary table:
  - `| Area | Decision | Reason |`

## Collision Rules

- If a mandatory property-driven region is hidden, force the structurally correct presentation variant instead of leaving a mismatched shell.
- If a region could be either `content-slot` or `property-driven-region`, choose `property-driven-region` unless the contract clearly requires consumer-editable composed content.
- If a region could be either `content-slot` or `atomic-child-component`, choose `atomic-child-component` unless the parent explicitly owns an editable body region.
- If a component could be either atoms-only composition or a guided parent molecule, choose the guided parent when designers normally start from the whole component and common use would otherwise require detaching.

## Validation

Before finalizing:

- Verify all candidate regions are classified before the first write
- Verify all required properties exist on the component set
- Verify property bindings are connected to relevant child nodes
- Verify property-driven structural behavior is reachable via property toggles rather than separate components
- Verify true `content-slot` regions remain editable body areas rather than fixed parent structure
- Verify decorative/internal regions were not promoted to `content-slot`
- Verify reusable child visuals are backed by child component instances rather than loose vectors
- Verify instance-swap properties are used only when the contract is genuinely swap-driven
- Verify atomic child documentation remains readable in its own section:
  - do not accept a child section where contrast-sensitive child variants disappear into the preview surface
  - decide and preserve a preview surface treatment that makes the child contract reviewable before the parent is marked complete
