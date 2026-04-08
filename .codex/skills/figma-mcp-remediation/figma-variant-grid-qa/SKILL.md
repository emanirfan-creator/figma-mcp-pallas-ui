---
name: figma-variant-grid-qa
description: Enforces deterministic variant grid layout and dedicated QA on separate light/dark testing pages. Use to prevent overlapping variants, missing spacing, and QA/test placement issues.
---

# Authoring Layout And QA

## When to use
- Variants inside a component set are overlapping or too tightly packed.
- After `combineAsVariants`, the variants are not reflowed into a visible grid with consistent spacing.
- QA frames are being placed on the component authoring root page instead of dedicated testing pages.
- Light/dark token testing is missing or only one mode is validated.
- The documentation area feels unfinished because it is not wrapped in a real frame/shell or has no intentional background/surface treatment.
- The documentation shell uses the wrong layout model and labels/headers drift away from the intended grid.

Load with `figma-use` and `figma-generate-library`.

## Goal

Produce readable, review-ready component pages for any component family. This is the canonical authoring-layout and QA skill; use `figma-dark-mode-semantic-qa` only when a focused dark-mode semantic investigation is still needed.

- Variants are laid out in visible grids
- The containing shell/frame is large enough to show the full documented grid
- Variant groupings are labeled like a design system documentation page
- Nothing overlaps
- QA runs on dedicated test pages in both light and dark modes
- QA proves the full supported variant and property matrix rather than one representative example

## Mandatory Rules

1. After `combineAsVariants`, always reflow children into a grid.
2. Add spacing between variant items (minimum 8px gap).
3. After any manual child repositioning, resize the `ComponentSetNode` to the furthest child bounds before validating screenshots or declaring the page readable.
4. Wrap the variant grid in a shell/frame sized to fully expose every variant and its labels.
5. Treat documented usage and hybrid layouts with the same shell requirement:
   - use a real frame as the documentation shell
   - give the shell an intentional surface treatment (background fill and, when useful, stroke/shadow)
   - keep section title/subtitle content inside that shell when it belongs to the documented section
6. Choose the shell layout model deliberately:
   - use fixed positioning when the shell needs labeled rows, labeled columns, or manually aligned sections
   - use auto-layout only when the shell is truly linear and grid alignment will not drift
7. Add documentation labels for each row or grouping and place them to the left when using row-based layouts.
8. Ensure all top-level frames/components are positioned away from existing content.
9. Validate top-level authoring-page sibling spacing and non-overlap before moving to testing-page QA.
10. Screenshot the authoring page after layout validation and before testing-page QA.
11. Keep component authoring page separate from QA pages.
12. Maintain two persistent QA pages:
   - `Light Mode Testing`
   - `Dark Mode Testing`
13. Append QA frames instead of replacing prior QA history.
14. When a component has variant axes, enumerate the intended QA matrix before placing instances.
15. Do not stop at one sample instance when multiple variant combinations are part of the supported contract.
16. For each required mode, place live QA that covers every required variant combination, unless preflight explicitly records an approved reduced matrix.
17. When `BOOLEAN`, `TEXT`, or `INSTANCE_SWAP` properties materially change usability, include the important non-default property permutations in the QA matrix.
18. If the supported matrix is too large for one frame, split it into multiple clearly named QA frames rather than silently reducing coverage.

## Validation Order

Run this sequence explicitly:

1. reflow the component-set children into the intended grid
2. resize the `ComponentSetNode` to the furthest child bounds
3. resize the documentation shell to the measured content bounds plus intended padding
4. validate top-level authoring-page spacing and non-overlap
5. screenshot the authoring page
6. enumerate the required variant/property QA matrix
7. only then proceed to light/dark testing-page QA
8. confirm every required matrix row was exercised in every required mode

## Grid Rules

- Columns: 4 default
- Item gap: 8px minimum
- Container padding: 24px
- Row gap: 16px default
- Reserve a left label column when documenting row-based variants
- Left label column width: 120px minimum, expand for longer labels
- Gap between label column and first variant cell: 16px default
- Resize the component set after layout so its own bounds fully cover the documented child grid
- Resize the outer shell after layout so no variant is clipped
- No overlap allowed between any siblings
- Page-level frames use horizontal offset placement to avoid collisions
- Treat shell hugging as measurable geometry, not a subjective polish step:
  - compare the outer shell bounds to the documented content extents
  - preserve the intended padding without leaving accidental empty overhang or clipped content

## Documentation Rules

- Use human-readable labels that match the actual variant contract, such as `Large` or `Borderless`
- Keep labels aligned in a dedicated left column
- Preserve enough horizontal gap between labels and the first variant cell for easy scanning
- Prefer consistent row titles over floating text annotations
- Use one documented row pattern repeatedly so the page reads like a design system spec, not an ad hoc canvas
- Do not treat a bare auto-layout frame with removed fills as a finished documentation shell unless a different surface treatment clearly communicates intentional presentation
- Validate that the documentation shell itself feels review-ready, not just structurally correct
- If labels, headers, and variants need fixed alignment, do not rely on the shell's auto-layout flow to preserve that relationship after the full grid is placed
- If documenting an atomic child component or child set, validate that the preview surface keeps the child readable across all documented variants
- Prefer `Size` as the row-label axis when present; place the secondary dimension across columns
- If `Size` is absent, choose the most readable primary grouping axis and keep it stable across the set
- Order values conventionally and predictably instead of alphabetically when the contract implies a clearer sequence
- Validate metadata after reflow: compare the component-set width/height to the maximum child extents, not just the shell frame size

## QA Placement Rules

- Create one QA frame per component per mode
- Place QA frames on testing pages only
- Use consistent baseline Y and computed X offsets
- Validate with screenshot after each mode frame is added
- Require at least one screenshot of a live component instance in each required mode
- Require live QA coverage for every supported variant combination in each required mode, unless a reduced matrix was explicitly approved during preflight
- Treat QA planning as a matrix:
  - variant axes across columns/rows or grouped frames
  - important property permutations as explicit rows
  - required modes duplicated across light and dark
- Keep behavioral QA rows comparable across modes; do not let one row silently change size, shape, or intent between light and dark examples unless the row label explicitly calls that out.
- Prefer readability-first behavioral QA examples in dark mode; avoid using low-contrast variants as the only proof that icon, loading, or slot behavior works.
- If a live-instance QA row fails in a required mode, append a separate fallback QA frame only after preserving the failing proof and explicitly labeling the fallback as non-primary evidence.
