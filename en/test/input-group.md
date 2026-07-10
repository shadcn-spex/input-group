# INPUTGROUP: Input Group

## Intent

Acceptance tests for the shadcn/ui `input-group` component, verifying the user contract in [../user/input-group.md](../user/input-group.md) and the implementation requirements in [../dev/input-group.md](../dev/input-group.md).
Each item maps Given-When-Then to GEARS (Given → Where/While, When → When, Then → shall).

## Items

### INPUTGROUP-18

Verifies: [INPUTGROUP-1](../user/input-group.md#inputgroup-1), [INPUTGROUP-10](../dev/input-group.md#inputgroup-10), [INPUTGROUP-31](../dev/input-group.md#inputgroup-31)

Where an `InputGroup` contains an `InputGroupInput` and an `InputGroupAddon` holding an `InputGroupText`, when the tree renders, the DOM shall contain a `div[role="group"][data-slot="input-group"]` wrapping a `div[role="group"][data-slot="input-group-addon"]`, an `input[data-slot="input-group-control"]`, and a `span` for the text.
When the rendered container is inspected, it shall show exactly one rounded border on the container and no border on the input element.

### INPUTGROUP-19

Verifies: [INPUTGROUP-2](../user/input-group.md#inputgroup-2), [INPUTGROUP-11](../dev/input-group.md#inputgroup-11)

Where four groups render one `InputGroupAddon` each with `align` set to `inline-start`, `inline-end`, `block-start`, and `block-end`, when each group is inspected, each addon shall carry the matching `data-align` attribute and the bounding boxes shall match the contract: inline addons beside the control in a fixed-height row (first or last respectively), block addons full-width above or below the control in a column-layout group with auto height.
Where no `align` prop is given, the addon shall render with `data-align="inline-start"`.

### INPUTGROUP-20

Verifies: [INPUTGROUP-3](../user/input-group.md#inputgroup-3), [INPUTGROUP-14](../dev/input-group.md#inputgroup-14)

Where a group contains an `InputGroupInput` and an addon holding an icon and text, when the user clicks the addon's text (outside any button), `document.activeElement` shall become the group's `<input>` element.

### INPUTGROUP-21

Verifies: [INPUTGROUP-3](../user/input-group.md#inputgroup-3), [INPUTGROUP-14](../dev/input-group.md#inputgroup-14)

Where a group contains an `InputGroupInput` and an addon holding an `InputGroupButton` with a click handler, when the user clicks the button (or an icon inside it), the click handler shall fire and `document.activeElement` shall not be the `<input>` element.

### INPUTGROUP-22

Verifies: [INPUTGROUP-4](../user/input-group.md#inputgroup-4)

Where a focusable element precedes a group whose DOM order is control then addon-with-button, while that preceding element has focus, when the user presses `Tab` twice, focus shall land first on the control and then on the addon button.
While the addon button has focus, when the user presses `ArrowDown`, focus shall remain on the addon button.

### INPUTGROUP-23

Verifies: [INPUTGROUP-4](../user/input-group.md#inputgroup-4)

Where a group's DOM order is control then addon-with-button, while the addon button has focus, when the user presses `Shift+Tab`, focus shall move back to the control.

### INPUTGROUP-24

Verifies: [INPUTGROUP-4](../user/input-group.md#inputgroup-4), [INPUTGROUP-9](../user/input-group.md#inputgroup-9)

Where a group contains an uncontrolled `InputGroupInput`, while the input has focus, when the user types `abc`, the input's value shall become `abc`.

### INPUTGROUP-25

Verifies: [INPUTGROUP-4](../user/input-group.md#inputgroup-4), [INPUTGROUP-8](../user/input-group.md#inputgroup-8), [INPUTGROUP-12](../dev/input-group.md#inputgroup-12)

Where a `<form>` with a submit listener wraps a group whose addon holds an `InputGroupButton` (no `type` prop) with a click handler, while the button has focus, when the user presses `Enter`, the click handler shall fire and the submit listener shall not fire.
While the button has focus, when the user presses `Space`, the click handler shall fire again.
When the button is inspected, it shall have `type="button"`, `data-size="xs"`, and the ghost-variant and xs-size classes.

### INPUTGROUP-26

Verifies: [INPUTGROUP-5](../user/input-group.md#inputgroup-5), [INPUTGROUP-13](../dev/input-group.md#inputgroup-13)

Where a group contains an `InputGroupInput`, when the user focuses the input via `Tab` (keyboard focus makes it `:focus-visible`), the group container shall render the ring-color border and a 3px ring, and the input element itself shall render no focus ring.
When focus leaves the group, the container ring shall disappear.

### INPUTGROUP-27

Verifies: [INPUTGROUP-6](../user/input-group.md#inputgroup-6), [INPUTGROUP-13](../dev/input-group.md#inputgroup-13)

Where a group's `InputGroupInput` has `aria-invalid="true"`, when the group renders, the container shall render its border in the destructive color and no ring while the input is unfocused.
While the invalid input is focused via keyboard, the container's 3px ring shall render in the destructive tint.
Where the input lacks `aria-invalid`, the container shall render the default `--input` border color.

### INPUTGROUP-28

Verifies: [INPUTGROUP-7](../user/input-group.md#inputgroup-7)

Where a group renders with `data-disabled="true"` on the `InputGroup`, when its addon is inspected, the addon's computed opacity shall be 0.5.
Where the same tree renders without `data-disabled`, the addon's computed opacity shall be 1.

### INPUTGROUP-29

Verifies: [INPUTGROUP-9](../user/input-group.md#inputgroup-9)

Where an `InputGroupInput` renders with `value="fixed"` and an `onChange` spy, while the input has focus, when the user types `x`, the spy shall be called and the displayed value shall remain `fixed`.
When the `value` prop is then updated to `fixed-x`, the displayed value shall become `fixed-x`.

### INPUTGROUP-30

Verifies: [INPUTGROUP-1](../user/input-group.md#inputgroup-1), [INPUTGROUP-4](../user/input-group.md#inputgroup-4), [INPUTGROUP-10](../dev/input-group.md#inputgroup-10)

Where a group contains an `InputGroupTextarea` and a `block-end` addon, when the group renders, the textarea shall carry `data-slot="input-group-control"` and the container height shall exceed the fixed row height (h-9), sized to the textarea.
While the textarea has focus, when the user presses `Enter`, a line break shall be inserted into the textarea value and no other group behavior shall trigger.
