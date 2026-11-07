# `<ui-range>`

`<ui-range>` is a form-associated slider web component for selecting a numeric value or, with `multiple`, a numeric interval. It uses custom-rendered slider thumbs rather than a native `<input type="range">`.

## Import

```js
import ".../ui-web/ui-range.js";
```

## Usage

Set `min` and `max` to define the range. They default to `0` and `100`. Omit `step` for a continuous range; otherwise, set it to a positive number to quantize values.

```html
<ui-range min="0" max="10" step="2" value="4"></ui-range>
```

The `value` attribute initializes the selected value; in single-value mode it is not automatically updated as the user moves the slider. The `value` property reports the current value.

### Multiple values

Add `multiple` for two thumbs. The `value` attribute takes a comma-separated pair (`from,to`); the `valueFrom` and `valueTo` properties get or set the current endpoints.

```html
<ui-range min="0" max="100" step="1" multiple value="20,80"></ui-range>
```

The first endpoint cannot move past the second, and the second cannot move before the first.

## Attributes

| Attribute | Effect |
| --- | --- |
| `min` | Minimum selectable value. Defaults to `0`. |
| `max` | Maximum selectable value. Defaults to `100`. If `min` is greater than `max`, the component swaps them. |
| `step` | Step interval. If absent or falsy, pointer movement is continuous and keyboard movement uses a calculated increment. |
| `value` | Initial/currently assigned value attribute. Use a single number for one thumb or a comma-separated pair for `multiple`. The component does not rewrite it as the value changes. |
| `multiple` | Enables two-thumb interval selection. |
| `reverse` | Reverses the value direction: minimum is at the right for horizontal sliders and at the top for vertical sliders. |
| `vertical` | Displays a vertical slider. Set the host's height to control its usable length. |
| `disabled` | Disables user interaction. Form-disabled state, including a disabled containing fieldset, is reflected internally. |
| `stepper` | Displays one marker at each step when a valid number of markers can be created. |
| `highlight` | Shows a value bubble while dragging or focusing a thumb. |
| `highlight-prefix` | Text prepended to the value shown in the highlight. |
| `highlight-suffix` | Text appended to the value shown in the highlight. |
| `highlight-swap` | Places the highlight on the opposite side of the thumb. |
| `label-lines` | Displays lines connecting label positions to the track. |
| `label-swap` | Moves labels to the opposite side of the track. |

`min`, `max`, `step`, and `value` are observed attributes. The other attributes are read during connection and are not all synchronized to internal state if changed later; configure these options in markup before the component is connected.

## Properties

| Property | Behavior |
| --- | --- |
| `value` | Gets or sets the current single-thumb value. In `reverse` mode, the getter/setter expose values in the reversed visual orientation. |
| `valueFrom` | Gets or sets the lower/from endpoint in multiple mode. In single mode, it returns the same value as `valueTo`. |
| `valueTo` | Gets or sets the upper/to endpoint in multiple mode; in single mode, it is the selected value. |

Property setters parse string values as numbers, round to the nearest step when `step` is set, and clamp values to the min/max bounds. They update the display and form value but do not update the `value` attribute or dispatch `input`/`change` events.

```js
const range = document.querySelector("ui-range");

range.value = 6;

const interval = document.querySelector("ui-range[multiple]");
interval.valueFrom = 20;
interval.valueTo = 80;
```

## Events

Both events bubble and are composed:

```js
range.addEventListener("input", (event) => {
  console.log(event.target.value);
});

range.addEventListener("change", (event) => {
  console.log(event.target.value);
});
```

- `input` fires during pointer movement when the selected value changes. The implementation does not emit it for keyboard changes.
- `change` fires when a pointer drag ends with a changed value, or when the track is clicked or a keyboard action changes a value.
- Programmatic property updates do not dispatch either event.

## Interaction and keyboard

Drag a thumb to adjust it. Clicking the track moves a single thumb one small increment toward the clicked side; in multiple mode, it moves the applicable thumb one small increment toward the click. Keyboard interaction is available when a thumb has focus:

| Key | Horizontal slider | Vertical slider |
| --- | --- | --- |
| Arrow keys | Left/right adjust by one step (or the calculated small increment). | Up/down adjust by one step (or the calculated small increment). |
| `PageUp` / `PageDown` | Adjust by a larger increment. | Adjust by a larger increment. |
| `Home` / `End` | Move to minimum / maximum. | Move to minimum / maximum. |

In reverse mode, the visual direction is reversed. The displayed order of slotted labels is not automatically reversed; provide labels in the intended order yourself.

## Labels and custom thumbs

Default-slot child elements are treated as slider labels and placed along the track in their DOM order. At least two label elements are needed for labels to display. Labels are positioned evenly; they do not have to correspond one-to-one with the step values.

```html
<ui-range min="0" max="10" step="2" label-lines>
  <div>0</div>
  <div>2</div>
  <div>4</div>
  <div>6</div>
  <div>8</div>
  <div>10</div>
</ui-range>
```

Use `slot="thumb"` to supply a custom thumb element. In single-value mode, the first custom thumb is used; in `multiple` mode, up to two are used. The custom content does not receive pointer events; the component's internal thumb handles interaction. Size custom thumbs with the same thumb size variables as the built-in thumb.

## Forms

The component is form-associated. Set `name` on the `<ui-range>` host to include its form value in submission.

- Single mode submits the current selected number as text.
- Multiple mode submits a comma-separated `from,to` pair.
- Form reset returns the range to its minimum (or maximum when reversed).
- Disabled form state disables interaction.

## CSS customization

Set these variables on `<ui-range>` or an ancestor:

| Variable | Default |
| --- | --- |
| `--ui-range-track-on-background` | `var(--ui-default-background, hsl(0 0% 90% / 1))`; the stylesheet uses this for the unselected track in the normal forward direction. |
| `--ui-range-track-off-background` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))`; the stylesheet uses this for the selected segment in the normal forward direction. |
| `--ui-range-track-size` | `4px` |
| `--ui-range-track-offset` | `8px` |
| `--ui-range-track-padding` | `4px` |
| `--ui-range-stepper-height` | `12px` |
| `--ui-range-stepper-width` | `12px` |
| `--ui-range-stepper-radius` | `10rem` |
| `--ui-range-stepper-on-color` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-range-stepper-off-color` | `var(--ui-default-background, hsl(0 0% 90% / 1))` |
| `--ui-range-thumb-height` | `16px` |
| `--ui-range-thumb-width` | `16px` |
| `--ui-range-thumb-radius` | `10rem` |
| `--ui-range-thumb-background` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-range-thumb-border-style` | `none` |
| `--ui-range-thumb-border-width` | `0` |
| `--ui-range-thumb-border-color` | `var(--ui-default-background, hsl(0 0% 90% / 1))` |
| `--ui-range-thumb-box-shadow` | `none` |
| `--ui-range-label-line-size` | `16px` |
| `--ui-range-label-line-style` | `solid` |
| `--ui-range-label-line-width` | `2px` |
| `--ui-range-label-line-color` | `var(--ui-default-background, hsl(0 0% 90% / 1))` |
| `--ui-range-label-gap` | `4px` |
| `--ui-range-highlight-gap` | `8px` |
| `--ui-range-highlight-background` | `var(--ui-default-background, hsl(0 0% 90% / 1))` |
| `--ui-range-highlight-foreground` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-range-highlight-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-range-highlight-font-weight` | `var(--ui-font-weight-bold, bold)` |
| `--ui-range-highlight-padding` | `8px 16px` |
| `--ui-range-highlight-border-radius` | `var(--ui-border-radius-normal, 4px)` |
| `--ui-range-highlight-border-style` | `none` |
| `--ui-range-highlight-border-width` | `var(--ui-border-width, 1px)` |
| `--ui-range-highlight-border-color` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-range-highlight-box-shadow` | `none` |

Track size, offsets, and padding adapt to vertical orientation. Thumb height and width, as well as stepper height and width, swap roles for vertical sliders. Note that the implementation applies the `on` and `off` track custom properties opposite to their names in the normal forward direction; the default fallback colors still render the selected portion dark and the unselected portion light.
