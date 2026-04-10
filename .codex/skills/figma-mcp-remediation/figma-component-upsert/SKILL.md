---
name: figma-component-upsert
description: Enforces deterministic upsert/deduplication when creating or updating any Figma component set. Use to prevent rerun duplication and to reuse/modify existing assets instead of creating new ones.
---

# Component Upsert Remediation

## When to use
- You are creating or updating any design-system component set in Figma (any component family).
- A rerun creates duplicates (new component sets/pages) instead of updating the existing ones.
- You need deterministic idempotency: explicit naming and "modify existing first".
- You want to ensure the agent does not place QA/test artifacts on the component's authoring root page.

Treat this as a targeted remediation overlay when rerun safety or existing-family refreshes are the real risk. Do not load it by default when standard implementation is already idempotent enough.

Load this skill together with `figma-use` and (when relevant) `figma-generate-library`.

## Goal

Prevent duplicate component sets and enforce deterministic updates:

- Reuse existing component/page/property assets whenever possible
- Update existing component sets instead of creating new copies
- Keep naming stable and explicit for idempotent reruns

## Mandatory Rules

1. Inspect before create:
   - Search pages and component sets by exact canonical name first.
2. Upsert-only component behavior:
   - If component set exists: update variants inside it.
   - If variant exists: overwrite child visuals and variable/style bindings.
   - If variant missing: create only the missing variant.
3. Never delete/recreate a component set for normal refreshes.
4. One page per component family:
   - Each component family gets a separate dedicated authoring page.
5. Keep testing artifacts off the component root page (use QA pages from companion skill).

## Naming Convention (Required)

Use deterministic names:

- Component set: canonical family name (example: `Button`, `Input`, `Select`)
- Variant instance names: `Property=Value, Property=Value`
- Component properties: stable semantic names (example: `Show Icon`, `Show Text`, `Label`, `Icon`)
- Page names: exact component name for authoring pages

Do not create ad-hoc suffixed names like `<Name> 2`, `<Name> Copy`, `New <Name>`.

## Upsert Decision Table

- Component page exists -> reuse page
- Component set exists -> reuse set
- Variant exists -> refresh variant internals
- Variant missing -> append variant
- Property exists -> reuse property name/hash
- Property missing -> create property once

## Return Requirements

Always return:

- Reused IDs
- Created IDs
- Updated IDs
- A dedupe report (`duplicatesFound`, `duplicatesRemoved`)
