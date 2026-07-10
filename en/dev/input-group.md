# INPUTGROUP: Input Group

## Intent

Implementation requirements for the shadcn/ui `input-group` component (new-york-v4 style), complementing the user contract in [../user/input-group.md](../user/input-group.md).
Ground truth is the registry source [[1]].

## Items

### INPUTGROUP-10

The module shall export exactly the six parts in the table below, each rendering the stated element with the stated attributes.

| Export | Renders | `data-slot` | Other attributes / defaults |
| --- | --- | --- | --- |
| `InputGroup` | `<div>` | `input-group` | `role="group"` |
| `InputGroupAddon` | `<div>` | `input-group-addon` | `role="group"`, `data-align={align}` |
| `InputGroupButton` | `Button` (registry) | inherited from `Button` | `data-size={size}`; defaults `type="button"`, `variant="ghost"`, `size="xs"` |
| `InputGroupText` | `<span>` | — | — |
| `InputGroupInput` | `Input` (registry) | `input-group-control` | overrides `Input`'s own `data-slot` |
| `InputGroupTextarea` | `Textarea` (registry) | `input-group-control` | overrides `Textarea`'s own `data-slot` |

Each part shall merge a consumer `className` after its own classes via `cn`, and shall spread all remaining props onto the rendered element.

### INPUTGROUP-11

The `InputGroupAddon` shall apply the `inputGroupAddonVariants` cva exactly as follows, with `defaultVariants: { align: "inline-start" }`.

Base classes:

```text
flex h-auto cursor-text items-center justify-center gap-2 py-1.5 text-sm font-medium text-muted-foreground select-none group-data-[disabled=true]/input-group:opacity-50 [&>kbd]:rounded-[calc(var(--radius)-5px)] [&>svg:not([class*='size-'])]:size-4
```

| `align` | Classes |
| --- | --- |
| `inline-start` | `order-first pl-3 has-[>button]:ml-[-0.45rem] has-[>kbd]:ml-[-0.35rem]` |
| `inline-end` | `order-last pr-3 has-[>button]:mr-[-0.45rem] has-[>kbd]:mr-[-0.35rem]` |
| `block-start` | `order-first w-full justify-start px-3 pt-3 group-has-[>input]/input-group:pt-2.5 [.border-b]:pb-3` |
| `block-end` | `order-last w-full justify-start px-3 pb-3 group-has-[>input]/input-group:pb-2.5 [.border-t]:pt-3` |

### INPUTGROUP-12

The `InputGroupButton` shall apply the `inputGroupButtonVariants` cva exactly as follows, with `defaultVariants: { size: "xs" }`, base classes `flex items-center gap-2 text-sm shadow-none`.

| `size` | Classes |
| --- | --- |
| `xs` | `h-6 gap-1 rounded-[calc(var(--radius)-5px)] px-2 has-[>svg]:px-2 [&>svg:not([class*='size-'])]:size-3.5` |
| `sm` | `h-8 gap-1.5 rounded-md px-2.5 has-[>svg]:px-2.5` |
| `icon-xs` | `size-6 rounded-[calc(var(--radius)-5px)] p-0 has-[>svg]:p-0` |
| `icon-sm` | `size-8 p-0 has-[>svg]:p-0` |

The `size` prop shall be typed by `inputGroupButtonVariants` (replacing `Button`'s own `size`), while `variant` shall pass through to `Button`.

### INPUTGROUP-13

The `InputGroup` container shall carry base classes `group/input-group relative flex w-full items-center rounded-md border border-input shadow-xs transition-[color,box-shadow] outline-none dark:bg-input/30 h-9 min-w-0` and shall apply the conditional classes in the table below via CSS `:has()` selectors, so the states require no JavaScript.

| Condition | Applied classes |
| --- | --- |
| `>textarea` direct child | `h-auto` |
| `>[data-align=inline-start]` direct child | `[&>input]:pl-2` |
| `>[data-align=inline-end]` direct child | `[&>input]:pr-2` |
| `>[data-align=block-start]` direct child | `h-auto flex-col [&>input]:pb-3` |
| `>[data-align=block-end]` direct child | `h-auto flex-col [&>input]:pt-3` |
| `[data-slot=input-group-control]:focus-visible` descendant | `border-ring ring-[3px] ring-ring/50` |
| `[data-slot][aria-invalid=true]` descendant | `border-destructive ring-destructive/20` (dark: `ring-destructive/40`) |

### INPUTGROUP-14

When a click event fires on an `InputGroupAddon` and `event.target.closest("button")` returns null, the addon's handler shall call `.focus()` on the first `<input>` returned by `parentElement.querySelector("input")`.
When `event.target.closest("button")` returns an element, the handler shall return without moving focus.

### INPUTGROUP-15

The module shall depend on exactly: npm package `class-variance-authority` (`cva`, `VariantProps`), React, the project's `cn` utility from `@/lib/utils`, and the registry dependencies `button`, `input`, and `textarea` (imported as `Button`, `Input`, `Textarea`).

### INPUTGROUP-16

The component classes shall consume only the semantic theme tokens in the table below, so a theme change re-skins the group without code changes.

| Token | Consumed via |
| --- | --- |
| `--input` | `border-input`, `dark:bg-input/30` |
| `--ring` | `border-ring`, `ring-ring/50` |
| `--destructive` | `border-destructive`, `ring-destructive/20`, `ring-destructive/40` |
| `--muted-foreground` | `text-muted-foreground` |
| `--radius` | `rounded-md`, `rounded-[calc(var(--radius)-5px)]` |

### INPUTGROUP-17

The parts shall compose under the following constraints; no part shall offer a `render` prop, and only `InputGroupButton` shall accept `asChild` (inherited from and forwarded to `Button`).

- `InputGroupAddon` and the control shall be direct children of `InputGroup`: the alignment, height, and input-padding selectors in [INPUTGROUP-13](#inputgroup-13) match direct children only.
- Consuming markup shall place `InputGroupAddon` after the control in DOM order; keyboard navigation relies on DOM order (see [INPUTGROUP-4](../user/input-group.md#inputgroup-4)).
- A custom control shall carry `data-slot="input-group-control"` to participate in the container focus and error styling of [INPUTGROUP-13](#inputgroup-13).
- `InputGroupButton` and `InputGroupText` shall be placed inside an `InputGroupAddon`; the addon's negative-margin and sizing selectors target its direct `button` and `kbd` children.

### INPUTGROUP-31

The `InputGroupInput` shall pass `Input` the override classes `flex-1 rounded-none border-0 bg-transparent shadow-none focus-visible:ring-0 dark:bg-transparent`, and the `InputGroupTextarea` shall pass `Textarea` the override classes `flex-1 resize-none rounded-none border-0 bg-transparent py-3 shadow-none focus-visible:ring-0 dark:bg-transparent`.
These classes shall make the control fill the remaining group space and suppress its standalone border, shadow, focus ring, and dark-mode background, so the container of [INPUTGROUP-13](#inputgroup-13) provides the only field chrome and the textarea is not manually resizable.

### INPUTGROUP-32

The `InputGroupText` shall carry the classes `flex items-center gap-2 text-sm text-muted-foreground [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4`.
The `pointer-events-none` on descendant `svg` elements shall make a click on an icon inside the text fall through to the surrounding addon, so the click-to-focus behavior of [INPUTGROUP-14](#inputgroup-14) applies.

## References

[1]: https://ui.shadcn.com/r/styles/new-york-v4/input-group.json "shadcn/ui registry item: input-group (new-york-v4)"
