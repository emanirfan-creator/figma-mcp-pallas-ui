---
name: figma-token-reconciliation
description: Maps code-level component token semantics to Figma variables before binding values. Use when implementing a tokenized component in Figma and exact code token names may not match the file's variable names, or when light/dark mode parity needs to be verified before finishing.
---

# Token Reconciliation

Load with `figma-use` and `figma-generate-library` before token-bound component writes.

## Goal

Part 1 of the token pipeline: resolve each required component token to a safe Figma variable mapping before any binding occurs.

## Classification

Every required token must be classified as one of:

- `exact`
- `substitute`
- `missing`
- `unsafe`

## Workflow

1. Extract required token semantics from the code source:
   - fill/background
   - border
   - text color
   - radius
   - spacing
   - size
   - typography
2. Inspect the Figma variable collections and text styles.
3. Map each required semantic token to a Figma variable or style.
4. Verify resolved values for any ambiguous scale token before binding:
   - compare expected code value to the candidate Figma variable value
   - do not rely on token path/name similarity alone for numeric scales such as spacing, sizing, radii, or control heights
5. Check mode coverage for every mapped color token.
6. Stop if any required mapping is `missing` or `unsafe`, unless the user explicitly approves the fallback.
7. If two or more plausible substitutes remain, ask the user to choose with `AskQuestion` instead of selecting one silently.

## Fallback Ladder

Use this decision order for every required token:

1. `exact`
   - The file contains the same semantic meaning, and the mapping remains correct in every required mode.
2. `substitute`
   - The exact token is missing, but one local semantic token is clearly the safest equivalent.
   - Report why the substitute is safe.
3. `unsafe`
   - Only primitive, visually similar, or meaning-shifting choices exist.
   - Do not bind without explicit user approval.
4. `missing`
   - No viable local token exists.
   - Stop and either ask for approval to create the token or ask the user to choose a fallback.

## Substitute Thresholds

Treat a substitute as safe to proceed only when all are true:

- the fallback preserves the same component role, not just a similar color
- the fallback behaves correctly across every required mode
- the fallback does not introduce a product-specific semantic shift
- only one clearly best local semantic choice exists

If any of the above is false, use `AskQuestion` instead of silently choosing.

## Required Output

Return a token table with:

- `codeToken`
- `expectedMeaning`
- `figmaTarget`
- `classification`
- `modesVerified`
- `valueVerified`
- `notes`

## Hard Rules

- Do not bind by loose name similarity alone.
- Distinguish "token exists" from "token is semantically correct".
- Verify light/dark parity when the file has multiple color modes.
- Prefer semantic variables over primitive variables.
- If using a substitute, say why it is safe.
- If the safest mapping depends on product or design intent rather than evidence, use `AskQuestion`.
- For scale tokens, verify the resolved numeric value before binding whenever similarly named candidates could map to different values.
- Hand the approved mapping table to `figma-token-style-bindings` for actual binding; do not repeat that binding guidance here.

## Common Cases

- Exact code token missing, semantic equivalent exists:
  - classify as `substitute`, explain why
- Exact semantic token missing, but a neutral local semantic token is the only mode-safe choice:
  - classify as `substitute`, explain why it is safe and what semantic detail was lost
- Only primitive or visually similar token exists:
  - classify as `unsafe`
- Neutral token missing in one mode:
  - classify as `missing` or `unsafe`, not `exact`
- Multiple visually acceptable substitutes exist:
  - pause and use `AskQuestion` to get a human choice
- Surface/elevation tokens missing:
  - prefer a neutral semantic background substitute only if the file lacks any surface tier and the fallback is mode-safe
  - otherwise classify as `missing` or `unsafe`
