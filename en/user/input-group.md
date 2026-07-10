# INPUTGROUP: Input Group

## Intent

This package pins the user-visible behavioral contract of the official shadcn/ui `input-group` component (new-york-v4 style) [[1]].
The component groups a text control (input or textarea) with addons (icons, text, buttons) inside a single bordered field whose border reflects the control's focus and validity state.
Ground truth is the registry source [[2]]; upstream code is MIT-licensed, Copyright (c) 2023 shadcn.

## Items

### INPUTGROUP-1

When rendered, the `InputGroup` shall present one container with `role="group"` that draws a single rounded border and shadow around the control and all addons (plus a translucent container background in dark mode), with no separate border, background, or focus ring on the control itself.
Where the group's only control is an `InputGroupInput` and every addon is inline-aligned, the container shall render as a fixed-height (h-9) horizontal row filling the available width.
Where the group contains a `textarea` as a direct child, the container shall size its height to fit the textarea instead of the fixed row height.

### INPUTGROUP-2

Where an `InputGroupAddon` is rendered with an `align` value from the table below (default `inline-start`), the addon shall occupy the stated position and the group shall adopt the stated layout.

| `align` | Addon position | Group layout |
| --- | --- | --- |
| `inline-start` | First in visual order, at the inline start of the row, beside the control | Single horizontal row, fixed height |
| `inline-end` | Last in visual order, at the inline end of the row, beside the control | Single horizontal row, fixed height |
| `block-start` | First in visual order, a full-width row above the control | Vertical column, auto height |
| `block-end` | Last in visual order, a full-width row below the control | Vertical column, auto height |

### INPUTGROUP-3

When the user clicks an `InputGroupAddon` anywhere outside a `<button>`, the group's `<input>` control shall receive focus.
When the click target is a `<button>` inside the addon (or a descendant of one), the addon shall not move focus to the control, and the button's own activation behavior shall proceed.

### INPUTGROUP-4

When the user presses a key listed in the table below while the stated element has focus, the input group shall produce the stated outcome.
The input group shall not intercept or remap any keyboard input beyond the native behavior of its elements.

| Key | While | Outcome |
| --- | --- | --- |
| `Tab` | Focus is on the element before the group | Focus moves to the first focusable element inside the group in DOM order |
| `Tab` | The control has focus | Focus moves to the next focusable element in DOM order (an addon button when one follows the control; otherwise the next element after the group) |
| `Shift+Tab` | An addon button placed after the control has focus | Focus moves back to the control |
| `Enter` | An `InputGroupButton` has focus | The button's click handler fires; with the default `type="button"`, no form submission occurs |
| `Space` | An `InputGroupButton` has focus | The button's click handler fires |
| `Enter` | An `InputGroupTextarea` has focus | A line break is inserted into the textarea value (native behavior) |
| Printable characters | The control has focus | The characters are inserted into the control's value |
| Arrow keys | Any element inside the group has focus | Focus does not move between group parts; native caret movement applies inside the control |

### INPUTGROUP-5

While the group's control (the element with `data-slot="input-group-control"`) is in the `:focus-visible` state, the group container shall display the focus indicator: border in the ring color plus a 3px ring at 50% ring-color opacity.
While no control inside the group is in the `:focus-visible` state, the container shall display no focus ring.

### INPUTGROUP-6

Where any slotted element inside the group (an element with a `data-slot` attribute) has `aria-invalid="true"`, the group container shall render its border in the destructive color and shall adopt the destructive color at 20% opacity (40% in dark mode) as its ring color.
While the invalid group's control is not in the `:focus-visible` state, the ring shall have no width, so the destructive border alone marks the error.
While the invalid group's control is in the `:focus-visible` state, the 3px ring of [INPUTGROUP-5](#inputgroup-5) shall render in the destructive tint instead of the ring color.

### INPUTGROUP-7

Where the group container has `data-disabled="true"`, every `InputGroupAddon` shall render at 50% opacity.
Where the group container lacks `data-disabled="true"`, addons shall render at full opacity.

### INPUTGROUP-8

Where an `InputGroupButton` is rendered without explicit props, the button shall render as a ghost-variant button at the `xs` size (h-6) with `type="button"`.
When such a button is activated inside a `<form>`, the form shall not be submitted.

### INPUTGROUP-9

Where an `InputGroupInput` or `InputGroupTextarea` receives a `value` prop with an `onChange` handler, the control shall behave as a controlled element: its displayed value shall equal the `value` prop, and user edits shall raise `onChange` without changing the displayed value until the prop changes.
Where the control receives a `defaultValue` prop or no value prop, the control shall manage its own value natively (uncontrolled).

## References

[1]: https://ui.shadcn.com/docs/components/radix/input-group "shadcn/ui — Input Group"
[2]: https://ui.shadcn.com/r/styles/new-york-v4/input-group.json "shadcn/ui registry item: input-group (new-york-v4)"
