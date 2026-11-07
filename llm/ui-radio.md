# `<ui-radio>`

`<ui-radio>` is a form-associated custom radio control with a native radio input and an optional label. Give radios in the same choice group the same `name`; selecting one checks it and unchecks other matching radios.

## Import

```js
import ".../ui-web/ui-radio.js";
```

## Usage

```html
<ui-radio name="shipping" value="standard" checked>Standard</ui-radio>
<ui-radio name="shipping" value="express">Express</ui-radio>
<ui-radio name="shipping" value="pickup">Store pickup</ui-radio>
```

Text and markup in the default slot are displayed as the label. The label is optional. Clicking the label area activates the radio.

## Attributes

| Attribute | Effect |
| --- | --- |
| `name` | Identifies the radio choice group. Use the same name for mutually exclusive options. |
| `value` | Value associated with this option and used for form submission when selected. |
| `checked` | Initializes or reflects the checked state on the internal native radio input. |
| `disabled` | Disables interaction when the form-associated custom element is disabled, including when disabled by a containing fieldset. |

The component observes `checked` and `value`. The component does not observe `name`; set the name in the markup or on the host before selection so the component's custom group-unchecking logic can find sibling radios.

## Properties

| Property | Type | Behavior |
| --- | --- | --- |
| `checked` | Boolean | Gets or sets the internal input's checked state. Setting a changed value dispatches a bubbling, composed `change` event. Setting it to `true` also unchecks other same-name radios found by the component. |
| `value` | String | Gets or sets the internal input's value. Assigning a falsy value clears the internal input value. |

```js
const radio = document.querySelector('ui-radio[name="shipping"]');

radio.checked = true;
radio.value = "standard";
```

The component does not expose a `disabled` property or a separate `name` property; use the corresponding host attributes.

## Events

Listen for `change` on the `<ui-radio>` element:

```js
radio.addEventListener("change", (event) => {
  console.log(event.target.checked, event.target.value);
});
```

The component emits a bubbling, composed `change` event when the user changes the internal radio or when code changes checkedness through the `checked` property. A programmatic checkedness update can produce change events both for the radio being checked and for another same-name radio that becomes unchecked. The event is a plain `Event` without custom `detail`.

## Forms and grouping

The component is form-associated. Put `name` and `value` on each `<ui-radio>` host for form submission:

```html
<form>
  <ui-radio name="plan" value="basic">Basic</ui-radio>
  <ui-radio name="plan" value="pro">Pro</ui-radio>
</form>
```

The component sets its form value when its internal radio changes and supports the browser's form-disabled, form-reset, and state-restoration callbacks. On form reset it clears the checked state. The custom group-unchecking logic searches matching `[name="..."]` elements while walking the containing DOM and shadow/slot ancestry; keep related controls in the same relevant tree.

## CSS customization

Set CSS custom properties on `<ui-radio>` or an ancestor:

| Variable | Default / fallback |
| --- | --- |
| `--ui-radio-accent-color` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-radio-label-foreground` | `var(--ui-foreground, hsl(0 0% 18% / 1))` |
| `--ui-radio-label-disabled-foreground` | Mix of foreground (25%) and background (75%) in `srgb-linear` |
| `--ui-radio-size` | `16px` |
| `--ui-radio-padding` | `2px` |
| `--ui-radio-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-radio-font-weight` | `var(--ui-font-weight-normal, normal)` |
| `--ui-radio-focus-width` | `3px` |
| `--ui-radio-focus-color` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-radio-gap` | `8px` |

```css
ui-radio {
  --ui-radio-accent-color: rebeccapurple;
  --ui-radio-size: 20px;
  --ui-radio-gap: 0.75rem;
  --ui-radio-label-foreground: #222;
}
```

`--ui-radio-size` changes the native radio dimensions and the surrounding focus area. `--ui-radio-padding` adds space around that area, and the focus variables control its background color and inset padding.
