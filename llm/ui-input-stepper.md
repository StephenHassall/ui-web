# `<ui-input-stepper>`

`<ui-input-stepper>` is a form-associated numeric range control. Users change the value in steps with buttons or keyboard controls. It displays the current number with optional prefix and suffix text.

## Import

```js
import ".../ui-web/ui-input-stepper.js";
```

## Basic usage

```html
<form>
  <label for="volume">Volume</label>
  <ui-input-stepper
    id="volume"
    name="volume"
    min="0"
    max="10"
    step="2">
  </ui-input-stepper>
  <button type="submit">Save</button>
</form>
```

The value is clamped between the minimum and maximum. When `step` is set, values are rounded to the nearest step from `min`. The default range is 0–100; with no `step`, the control uses 20 equal steps across the range. If `min` exceeds `max`, the component swaps the bounds.

## Attributes

| Attribute | Effect |
| --- | --- |
| `min` | Minimum selectable value. Defaults to `0`. |
| `max` | Maximum selectable value. Defaults to `100`. |
| `step` | Step size. When omitted or zero, the component divides the range into 20 increments. |
| `value` | Starting value. Defaults to `min`. |
| `prefix` | Text displayed before the numeric value. |
| `suffix` | Text displayed after the numeric value. |
| `arrows` | Uses directional chevrons instead of plus/minus glyphs for the separate controls. |
| `group-arrows` | Groups the increase/decrease controls together. |
| `vertical` | Places the separate controls above and below the value; with `group-arrows`, uses the alternate vertical layout. |
| `reverse` | Reverses which control increases/decreases and the corresponding visual direction. |
| `switch` | Moves the grouped controls to the opposite side. Applies to `group-arrows`. |
| `disabled` | Prevents interaction when applied through the associated form's disabled state. |
| `valid` / `invalid` | Applies shared input presentation styles; validity is not automatically determined. |

`arrows`, `group-arrows`, `vertical`, `reverse`, `switch`, `prefix`, and `suffix` are read when the component connects. Configure them in markup or before connection.

## Value property and form behavior

The `value` JavaScript property gets or sets the current numeric value:

```js
const stepper = document.querySelector("ui-input-stepper");
stepper.value = 6;
console.log(stepper.value);
```

String values are parsed as numbers. Setting a value clamps it to `min`/`max` and rounds it to the nearest configured step. The component is form-associated; give it a `name` attribute so its value is included in form submission. Form reset restores the value to `min`. The component supports browser form state restoration and disabled fieldsets.

Decimal precision in the displayed and submitted value is derived from the decimal places in `min`, `max`, and `step`.

## User interaction

- Clicking the decrement/increment control changes the value by one step.
- Arrow keys change the value by one step, with direction affected by layout and `reverse`.
- `PageUp` and `PageDown` change the value by a larger step.
- `Home` and `End` move to the minimum and maximum values.
- A `change` event bubbles and is composed whenever a user action actually changes the value.

With a configured `step`, the larger keyboard step is approximately one tenth of the range in step units, and is at least two normal steps. Without a configured step, it is one tenth of the range.

The focusable control exposes the slider role and its minimum, maximum, current value, and orientation through ARIA attributes.

```js
stepper.addEventListener("change", (event) => {
  console.log("New value:", event.target.value);
});
```

## CSS customization

Set these component-specific custom properties on `<ui-input-stepper>` or an ancestor:

| Variable | Default |
| --- | --- |
| `--ui-input-stepper-button-background` | `var(--ui-default-background, hsl(0 0% 90% / 1))` |
| `--ui-input-stepper-button-foreground` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-input-stepper-button-gap` | `6px` |
| `--ui-input-stepper-value-color` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-input-stepper-value-gap` | `4px` |

Shared input variables also style the control, including `--ui-input-background`, `--ui-input-foreground`, `--ui-input-border-radius`, `--ui-input-border-color`, and `--ui-input-padding`.

```css
ui-input-stepper {
  --ui-input-stepper-button-background: rebeccapurple;
  --ui-input-stepper-button-foreground: white;
  --ui-input-stepper-button-gap: 8px;
  --ui-input-stepper-value-color: #222;
  --ui-input-stepper-value-gap: 6px;
}
```
