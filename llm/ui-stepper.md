# `<ui-stepper>`

`<ui-stepper>` is a read-only progress indicator for showing the current position in a sequence of steps. It can display step numbers, state icons, labels, and optional per-label information in horizontal or vertical layouts.

## Import

```js
import ".../ui-web/ui-stepper.js";
```

## Usage

Set `steps` to the number of steps and `value` to the current one. Step values are 1-based: earlier steps are `done`, the matching step is `active`, and later steps are `todo`.

```html
<ui-stepper steps="4" value="2" numbers>
  <div>Account</div>
  <div>Profile</div>
  <div>Preferences</div>
  <div>Review</div>
</ui-stepper>
```

When `steps` is omitted, the number of default-slot labels determines the step count. If there are no labels, `slot="icon"` children can determine the step count instead. When using `double-label`, provide two default-slot label elements for each step, in left-then-right order.

```html
<ui-stepper value="2" vertical double-label>
  <div>Step 1 left</div>
  <div>Step 1 right</div>
  <div>Step 2 left</div>
  <div>Step 2 right</div>
</ui-stepper>
```

For vertical steppers, set a height large enough for the steps and labels. `swap-label` moves the labels to the opposite side. An element with `slot="info"` is associated with the preceding default-slot label and shown alongside it in vertical mode.

## Attributes

| Attribute | Effect |
| --- | --- |
| `steps` | Number of steps. Must be between 1 and 999. If omitted, the component counts default-slot labels or, if there are none, `slot="icon"` elements. |
| `value` | Current 1-based step number. Earlier steps are done, the matching step is active, and later steps are todo. Defaults internally to `0` if omitted. |
| `vertical` | Lays out the steps vertically. Set an appropriate host height. |
| `numbers` | Displays each step's 1-based number. |
| `icons` | Displays state icons: check for done, question mark for active, and minus for skipped. A todo step has no built-in icon unless a custom state icon is supplied. |
| `circle` | Renders steps as outlined circles rather than filled discs. |
| `active-highlight` | Changes the active step to the opposite circle/disc treatment from the other steps. |
| `skipped` | Comma-separated 1-based step numbers to mark skipped, for example `skipped="2,4"`. Skipped state takes precedence over done/active/todo. |
| `double-label` | Displays two default-slot labels for each step, left label first and right label second. |
| `swap-label` | Moves labels to the other side of the steps. |

Use the named slots below for custom icons or extra label information; these children do not count as default-slot labels.

The component observes changes to `double-label`, `skipped`, `steps`, `swap-label`, and `value`. Set `vertical`, `numbers`, and `icons` before connection; changing them later does not rebuild the rendered layout. `circle` and `active-highlight` are CSS-driven and can be changed dynamically.

## Slots

| Slot | Purpose |
| --- | --- |
| Default | Label for a step; labels are assigned in order. |
| `info` | Extra content associated with the preceding default-slot label. Displayed in vertical mode. |
| `icon` | Per-step custom icons, in step order. These can be used without `icons`. |
| `done` | Custom icon used for done steps when `icons` is enabled. |
| `active` | Custom icon used for the active step when `icons` is enabled. |
| `skipped` | Custom icon used for skipped steps when `icons` is enabled. |
| `todo` | Custom icon used for todo steps when `icons` is enabled. |

Custom icons are cloned into the rendered steps. `done`, `active`, `skipped`, and `todo` icon slots replace the built-in state icons only when the `icons` attribute is present. A custom `todo` icon is needed if todo steps should show an icon in `icons` mode.

## Properties

| Property | Behavior |
| --- | --- |
| `value` | Gets the current value as a number. Setting it updates the `value` attribute, which refreshes the displayed state. String values are parsed as integers. |

```js
const stepper = document.querySelector("ui-stepper");

stepper.value = 3;
console.log(stepper.value);
```

There are no custom events or interaction methods. The component is a visual indicator and does not advance steps itself.

## Styling labels

The component adds the state class `done`, `skipped`, `active`, or `todo` to both its internal label wrapper and the original slotted label element. Style the original label content to distinguish states:

```css
ui-stepper > .done {
  color: green;
}

ui-stepper > .active {
  font-weight: bold;
}
```

## CSS customization

Set these variables on `<ui-stepper>` or an ancestor:

| Variable | Default |
| --- | --- |
| `--ui-stepper-step-size` | `calc(var(--ui-font-size-normal, 16px) * 1.75)` |
| `--ui-stepper-primary-color` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-stepper-secondary-color` | `var(--ui-default-background, hsl(0 0% 90% / 1))` |
| `--ui-stepper-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-stepper-font-weight` | `var(--ui-font-weight-bold, bold)` |
| `--ui-stepper-circle-style` | `solid` |
| `--ui-stepper-circle-width` | `4px` |
| `--ui-stepper-line-size` | `4px` |
| `--ui-stepper-line-gap` | `0` |
| `--ui-stepper-line-style` | `solid` |
| `--ui-stepper-label-gap` | `8px` |

Primary color is used for done, active, and skipped steps; secondary color is used for todo steps. Line styling follows the done/todo state. Number and label text may need separate styling for the desired appearance.
