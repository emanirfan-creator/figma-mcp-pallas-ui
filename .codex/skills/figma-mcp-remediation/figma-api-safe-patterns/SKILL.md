---
name: figma-api-safe-patterns
description: Encodes safe Figma Plugin API patterns for component-library writes. Use when creating or updating tokenized components in Figma and you need reliable patterns for variable binding, component-property wiring, rerun-safe updates, and error recovery.
---

# Figma API Safe Patterns

Load with `figma-use` before emitting `use_figma` code for component-library writes.

## Goal

Prevent avoidable Figma API failures by using known-safe patterns for variable binding, component properties, and rerun-safe updates.

## Safe Patterns

### Variable binding

- For `setBoundVariable`, pass the variable ID expected by that API path.
- For paint bindings, use `setBoundVariableForPaint(...)` and reassign the full paint array.
- Reapply bindings after replacing child nodes or refreshing variant internals.

### Component properties

- Never assume the reference key equals the visible property name.
- After adding a component property, resolve the actual returned/generated key.
- Wire child `componentPropertyReferences` using that resolved key.

### Rerun-safe updates

- Inspect the existing component set before mutation.
- Reuse existing variants when names match exactly.
- Only create missing variants.
- When refreshing a variant, rebuild internals carefully and then restore bindings.

### Error recovery

- If a `use_figma` write fails, stop and fix the script before retrying.
- Treat failed writes as atomic and re-check assumptions from the error surface.
- Validate the minimal changed surface before moving on.

## Required Checks Before Write

- variable-binding API usage is consistent
- property keys are resolved, not guessed
- rerun behavior is explicit
- post-write validation step is defined

## Return Expectations

When relevant, return:

- `bindingDecisions`
- `propertyKeysResolved`
- `reusedVariantIds`
- `createdVariantIds`
- `recoveryNotes`
