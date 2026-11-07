# `<ui-dialog>`

A modal dialog web component built around the native HTML `<dialog>` element. Dialogs are opened and closed through the `UiDialog` class's static methods, using each dialog's `name`.

## Import

```js
import UiDialog from ".../ui-web/ui-dialog.js";
```

The module's default export is the `UiDialog` class. The custom element is registered as `<ui-dialog>`. It renders an open shadow root containing a native modal dialog with header, body, and footer slots.

## Basic usage

```html
<ui-dialog name="confirm-delete" size="small">
  <div style="padding: 1rem;">
    Delete this item?
    <ui-button danger>Delete</ui-button>
    <ui-button>Cancel</ui-button>
  </div>
</ui-dialog>
```

```js
UiDialog.open("confirm-delete");
UiDialog.close("confirm-delete");
```

Give each dialog a unique `name` and use the same string with `open()` and `close()`. The `name` attribute is used by the static methods to find the dialog; the component does not declare it as an observed attribute.

If `open(name)` is called before a matching dialog has connected, the component stores the requested name and opens the dialog when a matching element connects.

## Slots

| Slot | Purpose |
| --- | --- |
| Default (unnamed) | Dialog body content. |
| `header` | Optional header content above the body. |
| `footer` | Optional footer content below the body. |

When header or footer elements are assigned, the component sets internal `has-header` or `has-footer` attributes. These are used to conditionally display divider bars.

## Static API

| Method | Behavior |
| --- | --- |
| `UiDialog.open(name)` | Opens the connected dialog whose `name` attribute matches. |
| `UiDialog.close(name)` | Starts closing the connected dialog whose `name` matches. |
| `UiDialog.getActive()` | Returns the class's active-dialog reference, or `null` if it is empty. |

The native dialog is opened modally with `showModal()`. Opening and closing are animated according to the host attributes and CSS. Closing is asynchronous when an animation runs: the native dialog is closed after the close animation ends.

## Attributes

| Attribute | Values / effect |
| --- | --- |
| `name` | Required for lookup by the static `open()` and `close()` methods. Use a unique name. |
| `locked` | Prevents closing through outside clicks or Escape. It does not prevent calling `UiDialog.close(name)`. |
| `size` | `small`, `medium`, `large`, or `full`. With no size, the modal fits its content up to the viewport width. |
| `location` | `top` or `bottom`; otherwise the modal is centered. |
| `bar` | Shows dividers when header/footer content is present. |
| `bar-header` | Shows a divider under an assigned header. |
| `bar-footer` | Shows a divider above an assigned footer. |
| `open-animation` | Enables opening animation. Values `fade`, `up`, `down`, and `scale` are documented; `up`, `down`, and `scale` animate the modal movement/scale, while the generic enabled case fades. |
| `close-animation` | Enables closing animation. Values `fade`, `up`, `down`, and `scale` are documented. |

The `size` and `location` options are styling attributes read directly by CSS; the JavaScript does not validate their values.

## Close behavior

- Clicking outside the modal's bounding rectangle closes it unless `locked` is present.
- Pressing Escape closes it unless `locked` is present.
- `UiDialog.close(name)` closes it regardless of `locked`.
- Closing removes the native dialog's event listeners and calls its `close()` method after the closing animation ends.

## CSS customization

Set custom properties on `<ui-dialog>` or an ancestor. They style the modal panel, backdrop, borders, header/footer dividers, and shadow.

| Variable | Default / fallback |
| --- | --- |
| `--ui-dialog-background` | `var(--ui-module-background, hsl(0 0% 97% / 1))` |
| `--ui-dialog-foreground` | `var(--ui-module-foreground, hsl(0 0% 18% / 1))` |
| `--ui-dialog-backdrop` | `var(--ui-module-backdrop, hsla(0, 0%, 97%, 0.25))` |
| `--ui-dialog-border-radius` | `var(--ui-border-radius-large, 6px)` |
| `--ui-dialog-border-style` | `var(--ui-module-border-style, none)` |
| `--ui-dialog-border-width` | `var(--ui-module-border-width, 0)` |
| `--ui-dialog-border-color` | `var(--ui-module-border-color, transparent)` |
| `--ui-dialog-bar-style` | `var(--ui-border-style, solid)` |
| `--ui-dialog-bar-width` | `var(--ui-border-width, 1px)` |
| `--ui-dialog-bar-color` | `var(--ui-module-border-color, transparent)` |
| `--ui-dialog-box-shadow` | `var(--ui-module-shadow, 0 1px 4px 0 rgb(from var(--ui-module-foreground, hsl(0 0% 18% / 1)) r g b / 50%))` |

Example:

```css
ui-dialog {
  --ui-dialog-background: white;
  --ui-dialog-foreground: #222;
  --ui-dialog-backdrop: rgb(0 0 0 / 40%);
  --ui-dialog-border-radius: 1rem;
  --ui-dialog-border-style: solid;
  --ui-dialog-border-width: 1px;
  --ui-dialog-border-color: #bbb;
}
```

The modal has a default outer margin. Header/body/footer padding is not added by the component; add padding to the slotted content as needed. Forced-colors and increased-contrast modes add a stronger modal border and remove the shadow.
