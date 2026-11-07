# Text-based input components

`<ui-input-text>` and its specialized variants provide single-line, form-associated inputs with a shared appearance and API. The variants use native HTML input types for text, email, number, password, telephone, and URL data.

## Import

Import the module for the tag you use. Each specialized module imports the shared implementation.

```js
import ".../ui-web/ui-input-text.js";
import ".../ui-web/ui-input-email.js";
import ".../ui-web/ui-input-number.js";
import ".../ui-web/ui-input-password.js";
import ".../ui-web/ui-input-telephone.js";
import ".../ui-web/ui-input-url.js";
```

Only import the modules needed by the page.

## Components

| Element | Native input type | Use |
| --- | --- | --- |
| `<ui-input-text>` | `text` | General single-line text. |
| `<ui-input-email>` | `email` | Email address input and browser-provided email keyboard/UI. |
| `<ui-input-number>` | `number` | Numeric input, including decimal values; supports `min`, `max`, and `step`. |
| `<ui-input-password>` | `password` | Concealed password input. |
| `<ui-input-telephone>` | `tel` | Telephone input and browser-provided telephone keyboard/UI. |
| `<ui-input-url>` | `url` | URL input and browser-provided URL keyboard/UI. |

These are separate custom elements sharing one implementation and visual style. Native input type behavior (for example, mobile keyboards and browser-specific controls) depends on the browser and device.

## Basic usage

```html
<form>
  <label for="email">Email address</label>
  <ui-input-email id="email" name="email" autocomplete="email"
    placeholder="you@example.com">
    <ui-icon-circle-plus slot="start"></ui-icon-circle-plus>
  </ui-input-email>
  <button type="submit">Continue</button>
</form>
```

The `start` and `end` slots accept icons and other supplementary content:

```html
<ui-input-text placeholder="Search">
  <ui-icon-circle-plus slot="start"></ui-icon-circle-plus>
  <ui-icon-search slot="end"></ui-icon-search>
</ui-input-text>
```

## Attributes

The shared implementation forwards these observed attributes to its inner native `<input>`:

| Attribute | Effect |
| --- | --- |
| `value` | Sets the initial value and is observed for changes. Editing the input does not update the host's `value` attribute. |
| `placeholder` | Placeholder text shown when empty. |
| `autocomplete` | Browser autofill hint; use `off` to disable autofill where supported. |
| `autocapitalize` | Mobile keyboard capitalization hint, such as `sentences`, `words`, or `characters`. |
| `autocorrect` | Autocorrection hint, such as `on` or `off`; support varies. |
| `spellcheck` | Enables or disables browser spellchecking. |
| `enterkeyhint` | Mobile keyboard action hint, such as `search`, `next`, or `done`. |
| `readonly` | Makes the inner input read-only. |
| `min`, `max`, `step` | Forwarded to the native input; primarily useful with `<ui-input-number>`. |

Give the custom element a `name` to include its value in form submission. Add `valid` or `invalid` to apply the shared visual state; the component does not perform application-level validation for you.

## Value and input access

The `value` property reads or sets the current inner input value. Assigning `null`, `undefined`, or an empty string clears it.

```js
const input = document.querySelector("ui-input-text");
input.value = "Hello";
console.log(input.value);

// Access native input APIs when needed:
input.inputElement.setSelectionRange(0, 3);
input.focus();
```

`inputElement` returns the inner `HTMLInputElement`, for native methods, selection APIs, and events not otherwise surfaced by the component.

## Events

The native inner input emits its `input` event as the user types. Listen on the custom element to observe it:

```js
input.addEventListener("input", (event) => {
  console.log(event.target.value);
});
```

When the native input fires `change`, the component updates its form-associated value and dispatches a bubbling, composed `change` event from the custom element:

```js
input.addEventListener("change", (event) => {
  console.log("Committed value:", event.target.value);
});
```

## Password visibility

`<ui-input-password>` adds `showPassword(show)` to toggle whether the entered value is visible:

```js
const password = document.querySelector("ui-input-password");
password.showPassword(true);  // show as text
password.showPassword(false); // conceal as password
```

## Forms

The components are form-associated custom elements. Their value is submitted under the host's `name`. They support form-disabled callbacks, browser form state restoration, and form reset; reset clears the value.

For email, number, telephone, and URL fields, use application validation appropriate to your requirements. The `valid` and `invalid` attributes only control presentation.

## Styling

The shared input CSS variables can be set on the component or an ancestor. The following defaults are defined by the common input styles:

| Variable | Default |
| --- | --- |
| `--ui-input-background` | `field` |
| `--ui-input-foreground` | `fieldText` |
| `--ui-input-placeholder` | A 70% background / 30% foreground color mix |
| `--ui-input-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-input-font-weight` | `var(--ui-font-weight-normal, normal)` |
| `--ui-input-padding` | `8px 8px` |
| `--ui-input-gap` | `8px` |
| `--ui-input-border-radius` | `var(--ui-border-radius-normal, 4px)` |
| `--ui-input-border-style` | `var(--ui-border-style, solid)` |
| `--ui-input-border-width` | `2px` |
| `--ui-input-border-color` | `var(--ui-midground, hsl(0 0% 90% / 1))` |
| `--ui-input-border-valid-color` | `hsl(120 80% 50% / 1)` |
| `--ui-input-border-invalid-color` | `hsl(0 80% 50% / 1)` |
| `--ui-input-focus-width` | `2px` |
| `--ui-input-focus-shadow-width` | `4px` |
| `--ui-input-focus-color` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-input-focus-shadow-color` | `var(--ui-selected-background, hsl(215 50% 90% / 1))` |
| `--ui-input-disabled-background` | Mix of theme foreground/background colors |
| `--ui-input-disabled-foreground` | Mix of theme foreground/background colors |
| `--ui-input-icon-size` | `16px` |
| `--ui-input-icon-padding` | `0` |
| `--ui-input-box-shadow` | `var(--ui-border-box-shadow, none)` |

There are additional variables for valid/invalid focus, outline, and icon colors; see the shared inputs CSS reference for the full set. Content in the `start` and `end` slots inherits the input's icon color, including valid/invalid styling.

```css
ui-input-text {
  --ui-input-background: white;
  --ui-input-foreground: #222;
  --ui-input-border-radius: 8px;
  --ui-input-focus-color: rebeccapurple;
  --ui-input-icon-size: 20px;
}
```
