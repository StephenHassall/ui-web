# `<ui-toast>` and toast notifications

`UiToast` is the static manager for displaying toast notifications. It creates a `<ui-toast>` host as needed and places toast items in one of six viewport locations. The built-in item types are `<ui-toast-message>` and `<ui-toast-alert>`; applications can also register custom toast elements.

## Import

```js
import UiToast from ".../ui-web/ui-toast.js";
```

Import the icon component too if passing an icon tag such as `"ui-icon-check"`.

## Quick start

```js
UiToast.addMessage("Saved successfully.");

UiToast.addAlert(
  "success",
  "SAVED",
  "Your changes have been saved.",
  "ui-icon-check",
  true,
  0,
  UiToast.LOCATION_TOP_RIGHT
);
```

The manager mounts its host in `document.body` when the first toast is requested. Calls made before the host connects are queued. Call after the document body exists.

Toasts stack in their selected location. The stack is not capped, so applications should avoid producing an unbounded number of notifications.

## Built-in notification methods

### `UiToast.addMessage(text, showClose, iconTag, timeout, location)`

Adds a simple message. An empty or falsy `text` is ignored.

| Argument | Meaning |
| --- | --- |
| `text` | Message text. |
| `showClose` | Whether to show a close button. Defaults to `false`. |
| `iconTag` | Optional custom-element tag name for an icon, e.g. `"ui-icon-warning"`. The icon element must be registered/imported. |
| `timeout` | Auto-remove delay in milliseconds. If omitted while `showClose` is false, defaults to 5000 ms. |
| `location` | One of the `UiToast.LOCATION_*` constants. Defaults to bottom center. |

### `UiToast.addAlert(type, title, text, iconTag, showClose, timeout, location)`

Adds a colored alert. Unlike `addMessage`, it does not reject a missing title or text; the alert hides whichever content is empty.

| Argument | Meaning |
| --- | --- |
| `type` | Color style: `default`, `selected`, `success`, `warning`, `danger`, `info`, or `highlight`. |
| `title` | Optional title text. |
| `text` | Optional main message text. |
| `iconTag` | Optional registered icon tag name. |
| `showClose` | Whether to show a close button. Defaults to `false`. |
| `timeout` | Auto-remove delay in milliseconds. If omitted while `showClose` is false, defaults to 5000 ms. |
| `location` | One of the `UiToast.LOCATION_*` constants. Defaults to bottom center. |

With either method, a close button removes the toast when clicked. Setting `showClose` to `true` while leaving `timeout` unspecified means the toast has no automatic timeout.

## Timeout behavior

`timeout` is processed only when it is between 2500 and 60000 milliseconds inclusive. `0` means no automatic removal. Other out-of-range values are also ignored, so the toast remains until manually removed. Use `2500`–`60000` for an automatic timeout.

## Locations

Use the exported static numeric constants; they map to:

| Constant | Value | Position |
| --- | ---: | --- |
| `UiToast.LOCATION_TOP_LEFT` | `0` | Top left |
| `UiToast.LOCATION_TOP_CENTER` | `1` | Top center |
| `UiToast.LOCATION_TOP_RIGHT` | `2` | Top right |
| `UiToast.LOCATION_BOTTOM_LEFT` | `3` | Bottom left |
| `UiToast.LOCATION_BOTTOM_CENTER` | `4` | Bottom center (default) |
| `UiToast.LOCATION_BOTTOM_RIGHT` | `5` | Bottom right |

Pass one of these constants rather than an arbitrary number; the implementation does not validate location values.

## Custom toast elements

Register a custom element and implement `initializeCallback(data)`. The manager creates the element, waits for its custom-element definition, calls the initializer if present, then displays it.

```js
class AppToast extends HTMLElement {
  initializeCallback(data) {
    this.textContent = data.text;
  }
}

customElements.define("app-toast", AppToast);

UiToast.add(
  "app-toast",
  { text: "Background sync complete." },
  5000,
  UiToast.LOCATION_BOTTOM_CENTER
);
```

Signature: `UiToast.add(elementTag, data, timeout, location)`. The `data` argument is passed to `initializeCallback`. The timeout follows the same 2500–60000 ms rule; `0` or an out-of-range value leaves the toast visible until removed.

`UiToast.remove(element)` removes a displayed toast and animates its holder closed. It does nothing if the toast host has not been created or the element is not in the active toast list.

## Lifecycle callbacks

These are static manager callbacks, not DOM events:

```js
function onToastAdded(element, data) {
  console.log("Toast added", element, data);
}

function onToastRemoved(element, data) {
  console.log("Toast removed", element, data);
}

UiToast.addEventListener("add", onToastAdded);
UiToast.addEventListener("remove", onToastRemoved);

// Later, unregister the same callback reference:
UiToast.removeEventListener("add", onToastAdded);
UiToast.removeEventListener("remove", onToastRemoved);
```

The `"add"` callback receives `(element, data)` after the toast element is initialized and inserted. The `"remove"` callback receives `(element, data)` after the closing transition removes its holder from the DOM.

## Alert appearance options

`addAlert` exposes alert type, title, text, icon, close button, timeout, and location. To use the alert's `outline` or `invert` options, call `UiToast.add("ui-toast-alert", data, timeout, location)` with `outline: true` and/or `invert: true` in `data`.

```js
UiToast.add(
  "ui-toast-alert",
  {
    type: "warning",
    title: "CHECK",
    text: "Review the connection settings.",
    iconTag: "ui-icon-warning",
    showClose: true,
    outline: true,
    invert: false
  },
  0,
  UiToast.LOCATION_BOTTOM_CENTER
);
```

The alert element has `role="alert"`, `aria-live="polite"`, and `aria-atomic="true"`. Message toasts use `aria-live="polite"`. Both built-in types have a close button with an accessible label when `showClose` is enabled.

## CSS customization

Set custom properties on `<ui-toast>` or an ancestor (commonly `:root`). Toasts are fixed to the viewport. The manager sets each toast host's location internally.

### Container spacing

| Variable | Default |
| --- | --- |
| `--ui-toast-inner-gap` | `8px` |
| `--ui-toast-outer-gap` | `16px` |

### Message toast

| Variable | Default |
| --- | --- |
| `--ui-toast-message-background` | `var(--ui-module-background, hsl(0 0% 97% / 1))` |
| `--ui-toast-message-foreground` | `var(--ui-module-foreground, hsl(0 0% 18% / 1))` |
| `--ui-toast-message-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-toast-message-font-weight` | `var(--ui-font-weight-normal, normal)` |
| `--ui-toast-message-padding` | `8px 16px` |
| `--ui-toast-message-border-radius` | `var(--ui-border-radius-large, 6px)` |
| `--ui-toast-message-border-style` | `var(--ui-module-border-style, none)` |
| `--ui-toast-message-border-width` | `var(--ui-module-border-width, 0)` |
| `--ui-toast-message-border-color` | `var(--ui-module-border-color, transparent)` |
| `--ui-toast-message-box-shadow` | `var(--ui-module-shadow, 0 1px 4px 0 rgb(from var(--ui-module-foreground, hsl(0 0% 18% / 1)) r g b / 50%))` |
| `--ui-toast-message-icon-size` | `24px` |
| `--ui-toast-message-icon-padding` | `8px` |
| `--ui-toast-message-close-icon-size` | `16px` |
| `--ui-toast-message-close-padding` | `8px` |
| `--ui-toast-message-bar-style` | `solid` |
| `--ui-toast-message-bar-width` | `1px` |
| `--ui-toast-message-bar-color` | 50% opacity of the module foreground |

### Alert toast

Each alert type has a background and foreground variable. The defaults use the corresponding global color variables:

| Type | Background | Foreground |
| --- | --- | --- |
| `default` | `--ui-toast-alert-default-background` → `var(--ui-default-background, hsl(0 0% 90% / 1))` | `--ui-toast-alert-default-foreground` → `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `selected` | `--ui-toast-alert-selected-background` → `var(--ui-selected-background, hsl(215 50% 90% / 1))` | `--ui-toast-alert-selected-foreground` → `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `success` | `--ui-toast-alert-success-background` → `var(--ui-success-background, hsl(104 50% 90% / 1))` | `--ui-toast-alert-success-foreground` → `var(--ui-success-foreground, hsl(104 50% 30% / 1))` |
| `warning` | `--ui-toast-alert-warning-background` → `var(--ui-warning-background, hsl(45 50% 90% / 1))` | `--ui-toast-alert-warning-foreground` → `var(--ui-warning-foreground, hsl(45 50% 30% / 1))` |
| `danger` | `--ui-toast-alert-danger-background` → `var(--ui-danger-background, hsl(0 50% 90% / 1))` | `--ui-toast-alert-danger-foreground` → `var(--ui-danger-foreground, hsl(0 50% 30% / 1))` |
| `info` | `--ui-toast-alert-info-background` → `var(--ui-info-background, hsl(190 50% 90% / 1))` | `--ui-toast-alert-info-foreground` → `var(--ui-info-foreground, hsl(190 90% 30% / 1))` |
| `highlight` | `--ui-toast-alert-highlight-background` → `var(--ui-highlight-background, hsl(302 50% 90% / 1))` | `--ui-toast-alert-highlight-foreground` → `var(--ui-highlight-foreground, hsl(302 50% 30% / 1))` |

| Variable | Default |
| --- | --- |
| `--ui-toast-alert-border-radius` | `var(--ui-border-radius-large, 6px)` |
| `--ui-toast-alert-border-style` | `var(--ui-border-style, solid)` |
| `--ui-toast-alert-border-width` | `var(--ui-border-width, 1px)` |
| `--ui-toast-alert-border-color` | The current alert foreground |
| `--ui-toast-alert-box-shadow` | `var(--ui-border-box-shadow, none)` |
| `--ui-toast-alert-padding` | `8px` |
| `--ui-toast-alert-info-padding` | `0 8px` |
| `--ui-toast-alert-title-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-toast-alert-title-font-weight` | `var(--ui-font-weight-bold, 600)` |
| `--ui-toast-alert-text-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-toast-alert-text-font-weight` | `var(--ui-font-weight-normal, normal)` |
| `--ui-toast-alert-icon-size` | `24px` |
| `--ui-toast-alert-close-icon-size` | `16px` |
| `--ui-toast-alert-close-padding` | `8px` |
| `--ui-toast-alert-close-radius` | `var(--ui-border-radius-normal, 4px)` |

The alert stylesheet currently uses `--ui-toast-message-icon-padding` for the alert icon's padding; `--ui-toast-alert-icon-padding` appears in the alert stylesheet's variable documentation but is not used by its icon rule.
