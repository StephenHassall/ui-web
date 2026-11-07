# `<ui-label>`

`<ui-label>` is a styled label wrapper for form controls. Its content is displayed inside an internal native `<label>`, and its `for` attribute can focus a related control. Add `required` to show a visual asterisk.

## Import

```js
import ".../ui-web/ui-label.js";
```

## Basic usage

```html
<ui-label for="user-name">User name</ui-label>
<ui-input-text id="user-name" name="userName" autocomplete="username"></ui-input-text>
```

The `for` value is the target control's `id`, without a `#`. When the label is clicked, the component searches the current DOM/shadow ancestry for a matching element, focuses it, and calls its `showPicker()` method if available.

Content may also be placed directly inside the label:

```html
<ui-label>Age</ui-label>
<ui-input-number name="age"></ui-input-number>
```

## Attributes

| Attribute | Effect |
| --- | --- |
| `for` | Identifies a related control by its `id`. Clicking the label focuses the control and invokes `showPicker()` if available. |
| `required` | Displays a decorative asterisk after the label text. It does not set the control's `required` attribute or enforce validation. |

When the target has a boolean `checked` property, clicking the label also toggles that property. This supports custom checkbox-like controls; use the control's own required/validation attributes as appropriate.

## CSS customization

Set these variables on `<ui-label>` or an ancestor:

| Variable | Default |
| --- | --- |
| `--ui-label-foreground` | `var(--ui-foreground, hsl(0 0% 18% / 1))` |
| `--ui-label-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-label-font-weight` | `var(--ui-font-weight-bold, 600)` |
| `--ui-label-padding` | `8px 4px` |
| `--ui-label-required-font-size` | `var(--ui-font-size-large, 24px)` |
| `--ui-label-required-padding` | `0 4px` |
| `--ui-label-required-color` | `hsl(0 80% 50% / 1)` |

```css
ui-label {
  --ui-label-foreground: #333;
  --ui-label-font-size: 1rem;
  --ui-label-padding: 4px 8px;
  --ui-label-required-color: crimson;
}
```

