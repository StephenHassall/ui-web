# `<ui-input-textarea>`

`<ui-input-textarea>` is a form-associated, multi-line text control. It wraps a native `<textarea>` with the project's shared input styling.

## Import

```js
import ".../ui-web/ui-input-textarea.js";
```

## Basic usage

```html
<form>
  <label for="message">Message</label>
  <ui-input-textarea
    id="message"
    name="message"
    rows="4"
    placeholder="Write a message...">
  </ui-input-textarea>
  <button type="submit">Send</button>
</form>
```

Set the starting text between the component tags; there is no `value` attribute for the initial value:

```html
<ui-input-textarea name="notes" rows="4">
  These are the starting notes.
</ui-input-textarea>
```

The initial text is trimmed before being copied into the native textarea. The default native textarea height is two rows. Additional text can be entered beyond the visible rows, with vertical scrolling.

## Attributes

These attributes are observed by the component and copied to its inner `<textarea>`:

| Attribute | Effect |
| --- | --- |
| `rows` | Sets the visible number of text rows. |
| `cols` | Sets the native textarea's column sizing hint. |
| `placeholder` | Placeholder shown when the textarea is empty. |
| `readonly` | Prevents editing while allowing the text to be selected and copied. |
| `autocomplete` | Browser autofill hint. |
| `autocapitalize` | Mobile keyboard capitalization hint, such as `sentences`, `words`, or `characters`. |
| `autocorrect` | Autocorrection hint; support varies. |
| `spellcheck` | Enables or disables browser spellchecking. |
| `enterkeyhint` | Mobile keyboard action hint. |
| `dirname` | Native form directionality submission attribute. |
| `wrap` | Native textarea line-wrapping behavior. |
| `step` | Forwarded to the textarea; the native textarea does not use this attribute. |

Add `valid` or `invalid` to apply the shared visual styles. These are presentation states; the component does not validate the content.

## Value and native element access

The `value` property gets or sets the current text. Assign an empty string to clear it. The `defaultValue` property reads the starting text. `textareaElement` exposes the inner `HTMLTextAreaElement` for native APIs and properties.

```js
const textarea = document.querySelector("ui-input-textarea");
textarea.value = "Updated message";

console.log(textarea.value);
console.log(textarea.defaultValue);

textarea.textareaElement.focus();
```

## Events

The inner textarea emits the native `input` event as the user types. When it emits `change`, the component updates its form-associated value and dispatches a bubbling, composed `change` event from the custom element.

```js
textarea.addEventListener("input", (event) => {
  console.log("Current text:", event.target.value);
});

textarea.addEventListener("change", (event) => {
  console.log("Changed text:", event.target.value);
});
```

## Forms

The component is form-associated. Set `name` to submit its text value. It supports disabled fieldsets, browser form state restoration, and form reset; reset clears the current value.

## Scrolling and resizing

The default textarea cannot be manually resized. These host attributes change the native textarea behavior:

| Attribute | Effect |
| --- | --- |
| `vertical-scroll` | Always show vertical scrolling. |
| `horizontal-scroll` | Always show horizontal scrolling. |
| `vertical-resize` | Allow resizing vertically. |

```html
<ui-input-textarea rows="5" vertical-resize>
  Resizable text area
</ui-input-textarea>
```

## Styling

The component uses the shared input CSS variables for colors, typography, padding, borders, focus, disabled presentation, and box shadow. Common variables include:

| Variable | Default |
| --- | --- |
| `--ui-input-background` | `field` |
| `--ui-input-foreground` | `fieldText` |
| `--ui-input-placeholder` | A 70% background / 30% foreground color mix |
| `--ui-input-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-input-font-weight` | `var(--ui-font-weight-normal, normal)` |
| `--ui-input-padding` | `8px 8px` |
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
| `--ui-input-box-shadow` | `var(--ui-border-box-shadow, none)` |

See the shared inputs CSS reference for the additional focus, outline, and disabled-state variables.

```css
ui-input-textarea {
  --ui-input-background: white;
  --ui-input-foreground: #222;
  --ui-input-padding: 12px;
  --ui-input-border-radius: 8px;
}
```

