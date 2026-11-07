# `<ui-checkbox>`

A form-associated checkbox web component with an optional label, checked and indeterminate state, and CSS-variable styling.

## Import

```js
import ".../ui-web/ui-checkbox.js";
```

The component is registered as `<ui-checkbox>`. It renders a native `<input type="checkbox">` and a `<label>` in an open shadow root. The component is form-associated and delegates focus to its shadow root.

## Basic usage

```html
<ui-checkbox>Enable notifications</ui-checkbox>
<ui-checkbox checked>Accept the terms</ui-checkbox>
<ui-checkbox disabled>Unavailable option</ui-checkbox>
```

Text and markup placed in the default slot appear as the label. The label is optional.

## Attributes

| Attribute | Effect |
| --- | --- |
| `checked` | Mirrors the checked attribute to the internal checkbox, including removal. Use the `checked` property to change current checkedness directly. |
| `value` | Sets the internal checkbox's value used for form submission. The property is also available as `value`. |
| `disabled` | The form-associated custom element's disabled callback disables its internal input and label interaction. |

The component explicitly observes only `checked` and `value`. Disabled handling is provided by the browser invoking the form-associated `formDisabledCallback` when the custom control is disabled.

The component mirrors `checked` and `value` attribute changes to the internal input, removing the corresponding internal attribute when the host attribute is removed. The form value is refreshed after either attribute changes.

## Properties

| Property | Type | Behavior |
| --- | --- | --- |
| `checked` | Boolean | Gets or sets the internal checkbox's checked state. Setting a changed value dispatches a bubbling, composed `change` event from the custom element. |
| `indeterminate` | Boolean | Gets or sets the native checkbox's indeterminate state. Setting a changed value dispatches a bubbling, composed `change` event from the custom element. |
| `value` | String | Gets or sets the internal input's value. Assigning a falsy value (including the empty string) clears it. |

Example:

```js
const checkbox = document.querySelector("ui-checkbox");

checkbox.checked = true;
checkbox.indeterminate = false;
checkbox.value = "weekly";
```

The `indeterminate` state is a visual/native input state separate from `checked`; it is not reflected by an HTML attribute. User interaction with a native indeterminate checkbox clears its indeterminate state as part of the browser's native checkbox behavior.

## Events

Listen for `change` on the custom element:

```js
checkbox.addEventListener("change", (event) => {
  console.log(event.target.checked);
});
```

The component dispatches a bubbling, composed `change` event when:

- The user changes the internal input.
- The user clicks the component's label/container, which toggles the internal checked state.
- Code changes `checked` or `indeterminate` through the corresponding property setter.

Property-setter and label/container events are manually dispatched from the custom element and do not originate from the internal input. The input's own click handler stops click propagation before it can reach the container handler. The manually dispatched event is a plain `Event`, not a native input `change` event with input-specific details.

## Forms

The component opts into form association with `static formAssociated = true` and calls `ElementInternals.setFormValue()`. The internal input has no `name`; provide the form field name on the `<ui-checkbox>` host as required for form-associated custom controls.

```html
<form>
  <ui-checkbox name="updates" value="email">Email updates</ui-checkbox>
</form>
```

The component submits its value only while checked; when unchecked, it clears the form value. This state is synchronized on connection, on native input changes, on clicks to the label/container, and when the `checked` or `value` property is changed.

On form reset, the component restores the internal checkbox's checkedness from its `checked` content attribute and synchronizes the form value. Browser state restoration stores checkedness and value separately, then restores both. State saved by older versions contains only the value, so the component falls back to restoring that value and treats non-null legacy state as checked.

## CSS customization

Set CSS custom properties on `<ui-checkbox>` or an ancestor, such as `:root`. The component styles the native checkbox, its focus container, and its label using these variables.

| Variable | Default / fallback |
| --- | --- |
| `--ui-checkbox-accent-color` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-checkbox-label-foreground` | `var(--ui-foreground, hsl(0 0% 18% / 1))` |
| `--ui-checkbox-label-disabled-foreground` | Mix of foreground (75%) and background (25%) in `srgb-linear` |
| `--ui-checkbox-size` | `16px` |
| `--ui-checkbox-padding` | `3px` |
| `--ui-checkbox-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-checkbox-font-weight` | `var(--ui-font-weight-normal, normal)` |
| `--ui-checkbox-focus-radius` | `var(--ui-border-radius-normal, 4px)` |
| `--ui-checkbox-focus-style` | `var(--ui-border-style, solid)` |
| `--ui-checkbox-focus-width` | `2px` |
| `--ui-checkbox-focus-color` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-checkbox-gap` | `8px` |

Example:

```css
:root {
  --ui-checkbox-accent-color: rebeccapurple;
  --ui-checkbox-size: 20px;
  --ui-checkbox-gap: 0.75rem;
  --ui-checkbox-label-foreground: #222;
}
```

The size variable controls both the native checkbox dimensions and the outer focus-container dimensions. The padding, radius, border style, width, and color variables style the focus container.

## Accessibility and behavior notes

- The label text is rendered in a separate `<label>` element, but the native input is not nested inside that label and the label has no `for` association. The component manually toggles the checkbox when its outer container is clicked.
- The component does not add a host `role`, `aria-checked`, or an accessible name when the default slot is empty. Use meaningful label text and verify accessible behavior in target browsers and assistive technologies.
- The checkbox uses the browser's native checkbox rendering and `accent-color`.
- Forced-colors and increased-contrast modes adjust the focus container styling; reduced-motion preferences disable its border transition.
