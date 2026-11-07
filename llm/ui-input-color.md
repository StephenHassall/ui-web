# `<ui-input-color>`

`<ui-input-color>` is a form-associated color input with a custom popup picker. It provides a color preview, a hue slider, and a saturation/lightness map. Optional controls expose alpha transparency and editable HSLA, RGBA, or hexadecimal fields.

## Import

```js
import ".../ui-web/ui-input-color.js";
```

## Basic usage

```html
<form>
  <label for="accent-color">Accent color</label>
  <ui-input-color id="accent-color" name="accentColor" value="#3366CC"></ui-input-color>
  <button type="submit">Save</button>
</form>
```

By default, the value is a solid color represented as `#RRGGBB`. Add `alpha` to include transparency and use `#RRGGBBAA`. The default color is black. The component has a default slot for its content plus `start` and `end` slots for supplementary controls or icons.

## Attributes

| Attribute | Effect |
| --- | --- |
| `value` | Sets the starting color. Only this attribute is observed for changes after the element is created. |
| `alpha` | Enables the transparency slider and alpha fields; the value is represented as `#RRGGBBAA`. |
| `inputs` | Shows editable HSLA, RGBA, and HEX controls inside the picker. A button cycles between the formats. |
| `default-input` | Uses the browser's native `<input type="color">` instead of the custom picker. Native color inputs do not support alpha in this component. |
| `left` | Aligns the popup to the left edge of the component. |
| `right` | Aligns the popup to the right edge of the component. |
| `valid` / `invalid` | Applies the corresponding input border and icon color styles. These are presentation states, not automatic validation. |

`left` and `right` affect the custom popup. With neither, it is centered beneath the input. The component also supports shared input CSS variables and start/end slot conventions described in the common input reference.

## Value and color properties

The `value` JavaScript property gets or sets the current hex color:

```js
const colorInput = document.querySelector("ui-input-color");
colorInput.value = "#3366CC";
console.log(colorInput.value); // "#3366CC"
```

The component also exposes read-only numeric properties:

| Property | Range / meaning |
| --- | --- |
| `red`, `green`, `blue` | RGB channels, 0–255. |
| `hue` | Hue in degrees, 0–360. |
| `saturation`, `lightness` | HSL percentages, 0–100. |
| `alpha` | Opacity from 0 to 1. |

The component is form-associated. Give it a `name` to include its hex value in form submission. It updates its form value as the color changes. It implements form-disabled, form-state-restore, and form-reset callbacks.

## Picker methods

| Method | Behavior |
| --- | --- |
| `showPicker()` | Opens the custom picker, or invokes the native picker when `default-input` is enabled. Does nothing when the picker is already open or disabled. |
| `hidePicker()` | Closes the custom picker. For the native picker, it removes and re-adds the internal color input as a workaround to close it. |

The custom picker also opens when the color preview is clicked. Pressing Space or Enter opens/closes it; Escape closes it. Arrow keys adjust the active map or slider control.

## Events

The component dispatches bubbling, composed events from the host:

- `input` fires as the color changes through the custom picker.
- `change` fires when the custom picker closes with a different value. In `default-input` mode, `change` is forwarded when the browser's color input commits a value.

```js
colorInput.addEventListener("input", (event) => {
  console.log(event.target.value);
});

colorInput.addEventListener("change", (event) => {
  console.log("Committed color:", event.target.value);
});
```

## CSS customization

The component uses shared input variables for its base appearance. Its color-picker-specific variables are:

| Variable | Default |
| --- | --- |
| `--ui-input-color-height` | `calc(var(--ui-input-font-size, var(--ui-font-size-normal, 16px)) * 1.5)` |
| `--ui-input-color-dialog-height` | `250px` |
| `--ui-input-color-dialog-height-alpha` | `25px` |
| `--ui-input-color-dialog-height-inputs` | `50px` |
| `--ui-input-color-dialog-width` | `250px` |
| `--ui-input-color-dialog-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-input-color-dialog-font-weight-bold` | `var(--ui-font-weight-bold, 600)` |
| `--ui-input-color-gap` | `8px` |
| `--ui-input-color-shift` | `2px` |

The picker height is composed from the base height plus the optional alpha and inputs heights, depending on enabled attributes. These extra heights determine the total popup size; they do not directly set each corresponding row's exact rendered height.

```css
ui-input-color {
  --ui-input-color-height: 2.5rem;
  --ui-input-color-dialog-width: 20rem;
  --ui-input-color-dialog-height: 18rem;
}
```

