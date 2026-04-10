# Pallas UI Component Mapping

This document defines how Pallas UI components should be interpreted when converting code concepts into Figma components and properties.

It is intended to help agents model each component consistently, especially when deciding:
- whether something is an atomic component or a molecule
- which regions should become text properties
- which optional regions should become boolean properties
- which composed parents should expose slots
- which child pieces should be treated as reusable atomic children

## Global Conventions

- Any visible text should be represented with a text property.
- Any optional text region should still use a text property, paired with a boolean property when the region itself can be shown or hidden.
- Variants in Figma represent the same component across predefined states or other stable configuration axes such as size, type, orientation, shape, or state.
- A boolean property in Figma should be used for any region that a designer may need to show or hide while using the design system.
- A text property should always be applied to any text used inside a component.
- Components should be classified explicitly as either `atomic` or `molecule`.
- Naming convention: a reusable nested inner child should be treated as an `atom`, while the composed nested parent that brings those children together should be treated as a `molecule`.
- A `molecule` should call out the atomic children it is composed from when that structure matters for conversion.
- A slot should be used when a component contains a region with variable nested content that cannot be represented as a fixed text property, boolean property, or predefined variant.
- Example: the body of a modal can be a slot because it may contain text, form fields, icons, illustrations, or other nested components.
- Optional icons, descriptions, helper text, labels, and similar conditional regions should be modeled as boolean properties unless stated otherwise.
- Typography should be implemented as text styles using tokens, not as Figma components.

## Component Mapping

## Accordion

- Level: `molecule`
- Purpose: show and hide additional content in an expandable section
- Built from: header region and expandable description/content region
- Variant axes:
  - `state`: `open | closed`
- Boolean properties:
  - `isOpen`
  - `hasDescription`
- Text properties:
  - `headerText`
  - `description`
- Slot placement:
  - expandable content region below the header
- Notes:
  - The closed state should expose the header text.
  - The opened state should expose the description/content region.

## Alert

- Level: `molecule`
- Purpose: surface important information or actions sparingly
- Built from: icon, header text, and description
- Variant axes:
  - `variant`: `info | warning | error | neutral`
- Boolean properties:
  - `hasIcon`
  - `hasDescription`
- Text properties:
  - `headerText`
  - `description`
- Notes:
  - Each alert variant includes an icon, a header, and a description by default.
  - The icon and description should still be controllable as optional regions.

## Avatar

- Level: `atomic`
- Purpose: represent a user visually
- Built from: image region or fallback user icon
- Variant axes:
  - `size`: `small | medium | large | xl`
  - `shape`: `circular | square`
- Boolean properties:
  - `hasImage`
  - `hasFallbackIcon`
- Notes:
  - The avatar should support either a user image or a fallback user icon.
  - The image and fallback icon should not be treated as simultaneously required.

## Badge

- Level: `atomic`
- Purpose: indicate status or emphasis
- Built from: text region and optional icon region
- Variant axes:
  - `color`: one value per palette color, for example `blue | red | green`, not every tonal variant
- Boolean properties:
  - `hasIcon`
- Text properties:
  - `label`

## Breadcrumb

- Level: `molecule`
- Purpose: show a navigation path through multiple inline items
- Built from:
  - atomic breadcrumb item
  - repeated inline composition of breadcrumb items
- Slot behavior:
  - the molecule should expose a slot so breadcrumb items can be added or removed
- Slot placement:
  - inline breadcrumb item list
- Atomic child components:
  - `Breadcrumb Item`
- `Breadcrumb Item` structure:
  - text region
  - left icon region
  - right icon region
- `Breadcrumb Item` boolean properties:
  - `hasText`
  - `hasLeftIcon`
  - `hasRightIcon`
- `Breadcrumb Item` text properties:
  - `label`

## Button

- Level: `molecule`
- Purpose: trigger actions
- Built from: text region and optional icon region
- Variant axes:
  - `size`: `sm | default | large`
  - `shape`: `rounded | not-rounded`
  - `state`: `default | hover | focused | disabled | dashed | ghost`
  - `type`: `primary | secondary | link`
- Boolean properties:
  - `hasIcon`
  - `hasText`
- Text properties:
  - `label`
- Notes:
  - Do not model a separate icon-only button in Figma.
  - Buttons should remain one component system with optional text and icon regions.

## Carousel

- Level: `molecule`
- Purpose: display content as a horizontal or vertical sequence with navigation
- Built from:
  - secondary arrow buttons
  - fixed image rectangle
  - pagination ellipse component
- Variant axes:
  - `orientation`: `horizontal | vertical`
- Boolean properties:
  - `hasPagination`
- Atomic child components:
  - secondary button with arrow icon
  - pagination ellipse
- Notes:
  - The image region is fixed content structure, not a reusable slot in this definition.

## Checkbox

- Level: `molecule`
- Purpose: allow binary or partial selection with optional label
- Built from:
  - label atom
  - checkbox box atom
- Boolean properties:
  - `hasLabel`
- Atomic child components:
  - `Label`
  - `Checkbox Box`
- `Checkbox Box` variant axes:
  - `state`: `checked | default | hover | disabled | indeterminate`
  - `size`: `sm | md | lg`

## Command

- Level: `molecule`
- Purpose: combine search and selectable command items
- Built from:
  - search atom
  - command item atoms
- Atomic child components:
  - `Command Item`
  - search component
- Slot placement:
  - stacked command item list below the search region
- `Command Item` variant axes:
  - `state`: `hover | selected | disabled`
- `Command Item` boolean properties:
  - `hasIcon`
  - `hasText`
- `Command Item` text properties:
  - `label`

## Combobox

- Level: `molecule`
- Purpose: combine an input field with command-style selection behavior
- Built from:
  - input field
  - command component
- Atomic child components:
  - input field
  - command
- Slot placement:
  - command/list content region associated with the input trigger

## Date Picker

- Level: `molecule`
- Purpose: select a date or date range
- Built from:
  - arrow button atoms
  - date picker item atoms
  - optional search atom in one variant
- Variant axes:
  - `type`: `default | date-range | with-search`
- Atomic child components:
  - arrow button
  - `Date Picker Item`
  - search
- Slot placement:
  - date item grid/body region
- `Date Picker Item` variant axes:
  - `state`: `selected | hover | disabled`

## Sheet

- Level: `molecule`
- Purpose: present overlay content with title, description, and actions
- Built from:
  - title
  - subtext
  - description
  - close button
  - secondary CTA
  - primary CTA
- Slot behavior:
  - the sheet should be modeled as a slot-driven parent so internal content can change
- Slot placement:
  - body content region between the header content and footer CTAs
- Atomic child components:
  - close button
  - secondary button
  - primary button
- Text properties:
  - `title`
  - `subtext`
  - `description`

## Form

- Level: `molecule`
- Purpose: compose multiple form controls into a reusable structure
- Built from:
  - input
  - checkbox
  - radio
  - button
- Slot behavior:
  - the form should expose a slot so controls can be arranged as needed
- Slot placement:
  - main stacked form content region
- Atomic child components:
  - input
  - checkbox
  - radio
  - button

## Input

- Level: `molecule`
- Purpose: collect user-entered text with optional adornments and guidance
- Built from:
  - field container
  - optional left icon
  - optional right icon
  - optional prefix atom
  - optional postfix atom
  - optional label atom
  - optional helper text
  - optional error message
- Variant axes:
  - `size`: `sm | md | lg`
  - `shape`: `rounded | default`
  - `status`: `success | error | warning`
  - `state`: `active | hover | default | focused | disabled`
- Boolean properties:
  - `hasLeftIcon`
  - `hasRightIcon`
  - `hasPrefix`
  - `hasPostfix`
  - `hasLabel`
  - `hasHelperText`
  - `hasErrorMessage`
- Text properties:
  - `value`
  - `label`
  - `helperText`
  - `errorMessage`
- Atomic child components:
  - `Label`
  - prefix atom
  - postfix atom

## Label

- Level: `atomic`
- Purpose: provide text labeling for controls
- Text properties:
  - `label`
- Notes:
  - This component is intended for reuse across controls.

## Input OTP

- Level: `molecule`
- Purpose: collect one-time passcode values across repeated item cells
- Built from:
  - repeated OTP item atoms
- Slot behavior:
  - the molecule should expose a slot so OTP items can be repeated as needed
- Slot placement:
  - inline OTP item sequence
- Atomic child components:
  - `OTP Item`
- `OTP Item` variant axes:
  - `style`: `outline | filled | underlined | borderless`
  - `status`: `error | success | warning`
  - `size`: `sm | md | lg`
  - `shape`: `default | round`

## Menu Bar

- Level: `molecule`
- Purpose: present a horizontal menu of selectable items with optional combobox support
- Built from:
  - menubar item atoms
  - combobox atom
- Slot behavior:
  - the menubar should expose a slot so items can be added, removed, or reordered
- Slot placement:
  - inline menubar item list
- Atomic child components:
  - `Menubar Item`
  - combobox
- `Menubar Item` variant axes:
  - `state`: `hover | selected | default`
- `Menubar Item` boolean properties:
  - `hasIcon`
- `Menubar Item` text properties:
  - `label`

## Popover

- Level: `molecule`
- Purpose: show contextual content anchored to a control
- Built from:
  - control region
  - heading
  - subheading
  - flexible content region
- Slot behavior:
  - the popover should expose a slot so content can be added or removed
- Slot placement:
  - popover body/content region below the heading and subheading
- Text properties:
  - `heading`
  - `subheading`

## Progress

- Level: `molecule`
- Purpose: show progress as a bar or circle with optional label
- Built from:
  - progress bar or circle atom
  - label atom
- Variant axes:
  - `size`: `sm | md | lg`
  - `orientation`: `horizontal | vertical`
  - `shape`: `circle | bar`
  - `status`: `indeterminate | default`
- Boolean properties:
  - `hasLabel`
- Atomic child components:
  - progress bar/circle atom
  - `Label`

## Radio Group

- Level: `molecule`
- Purpose: allow single selection with optional label
- Built from:
  - label atom
  - radio button atom
- Boolean properties:
  - `hasLabel`
- Atomic child components:
  - `Label`
  - `Radio Button`
- `Radio Button` variant axes:
  - `state`: `selected | default | hover | disabled | indeterminate`
  - `size`: `sm | md | lg`

## Segmented

- Level: `molecule`
- Purpose: switch between a small set of segmented options
- Built from:
  - segmented item atoms
- Variant axes:
  - `orientation`: `horizontal | vertical`
- Slot behavior:
  - the segmented control should expose a slot so segmented items can be repeated inline
- Slot placement:
  - segmented item list within the control container
- Atomic child components:
  - `Segmented Item`
- `Segmented Item` variant axes:
  - `state`: `hover | selected | disabled | default`
  - `size`: `sm | md | lg`
- `Segmented Item` boolean properties:
  - `hasIcon`
- `Segmented Item` text properties:
  - `label`

## Select

- Level: `molecule`
- Purpose: choose from a set of options through a select field
- Built from:
  - text label
  - select field
  - fixed right chevron-down icon
- Variant axes:
  - `size`: `sm | md | lg`
  - `shape`: `rounded | default`
  - `status`: `success | error | warning`
  - `state`: `active | hover | default | focused | disabled`
- Boolean properties:
  - `hasLabel`
  - `hasHelperText`
  - `hasErrorMessage`
- Text properties:
  - `label`
  - `value`
  - `helperText`
  - `errorMessage`
- Atomic child components:
  - `Label`
- Notes:
  - The chevron-down icon on the right is always present.
  - The select is defined by the combination of the text label and the field.

## Separator

- Level: `atomic`
- Purpose: visually divide content
- Variant axes:
  - `type`: `horizontal | vertical`
  - `style`: `dotted | solid | dashed | rounded`

## Skeleton

- Level: `molecule`
- Purpose: show loading placeholders for circular and text content
- Built from:
  - circle atom
  - text atom
- Atomic child components:
  - skeleton circle
  - skeleton text

## Slider

- Level: `molecule`
- Purpose: adjust a value along a track
- Built from:
  - progress-like track
  - handle atom
- Atomic child components:
  - progress bar/circle base
  - handle
- Notes:
  - The slider follows the progress component structure but adds a handle atom.

## Spinner

- Level: `molecule`
- Purpose: show loading activity with optional label
- Built from:
  - spinner item atom
  - label atom
- Atomic child components:
  - `Spinner Item`
  - `Label`
- `Spinner Item` variant axes:
  - `size`: `sm | md | lg`
  - `type`: `disabled | default`
- Text properties:
  - `label`

## Steps

- Level: `molecule`
- Purpose: show ordered progress through multiple steps
- Built from:
  - step item molecules
  - separator atom
- Variant axes:
  - `orientation`: `horizontal | vertical`
- Atomic child components:
  - `Step Item`
  - `Separator`
- `Step Item` built from:
  - step circle atom
  - label atom
- `Step Item` variant axes:
  - `state`: `selected | loaded | disabled | done`
  - `size`: `sm | md | lg`
- `Step Item` boolean properties:
  - `hasLabel`
- `Step Item` text properties:
  - `label`

## Switch

- Level: `molecule`
- Purpose: toggle between on and off states with optional label
- Built from:
  - label atom
  - toggle atom
- Boolean properties:
  - `hasLabel`
- Atomic child components:
  - `Label`
  - `Toggle`
- `Toggle` variant axes:
  - `state`: `on | off | disabled | enabled`

## Tabs

- Level: `molecule`
- Purpose: switch between grouped content panels using tab items
- Built from:
  - repeated tab item atoms
- Variant axes:
  - `orientation`: `horizontal | vertical`
- Slot behavior:
  - the tabs component should expose a slot so tab items can be added or removed
- Slot placement:
  - tab item list within the tabs container
- Atomic child components:
  - `Tab Item`
- `Tab Item` variant axes:
  - `state`: `selected | default | disabled`
  - `type`: `vertical | horizontal`
- `Tab Item` text properties:
  - `label`

## Text Area

- Level: `molecule`
- Purpose: collect multi-line user input
- Built from:
  - label atom
  - textarea item atom
- Boolean properties:
  - `hasLabel`
  - `hasHelperText`
  - `hasErrorMessage`
- Text properties:
  - `label`
  - `value`
  - `helperText`
  - `errorMessage`
- Atomic child components:
  - `Label`
  - `Textarea Item`
- `Textarea Item` variant axes:
  - `size`: `sm | md | lg`
  - `shape`: `rounded | default`
  - `status`: `success | error | warning`
  - `state`: `active | hover | default | focused | disabled`
  - `style`: `underlined | filled | borderless | outline`
  - `radius`: `sm | default | lg`

## Toast

- Level: `molecule`
- Purpose: show short-lived feedback with optional actions
- Built from:
  - heading
  - subtext
  - description
  - icon
  - close button
  - CTA
- Slot behavior:
  - the toast should expose a slot because the content inside can change
- Slot placement:
  - main toast body/content region
- Boolean properties:
  - `hasIcon`
- Text properties:
  - `heading`
  - `subtext`
  - `description`
- Atomic child components:
  - close button
  - CTA button

## Tooltip

- Level: `atomic`
- Purpose: display brief contextual help near another element
- Variant axes:
  - `position`: `top | right | bottom | left`
  - `alignment`: `center | end | start`
- Text properties:
  - `label`

## Typography

- Level: `not-a-component`
- Purpose: define semantic text styles for the system
- Notes:
  - Typography should be implemented as token-backed text styles.
  - Typography should not be modeled as reusable Figma components.
