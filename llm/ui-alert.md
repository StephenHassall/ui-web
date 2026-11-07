# `<ui-alert>`

A message panel for calling attention to status, warnings, or additional information. Alerts support status colors, icons, expandable content, right-aligned actions, and an optional close control.

## Import

```js
import ".../ui-web/ui-alert.js";
```

The component is registered as `<ui-alert>`. It renders its contents in an open shadow root.

## Basic usage

```html
<ui-alert>Changes saved.</ui-alert>

<ui-alert warning>
  <ui-icon-warning slot="icon"></ui-icon-warning>
  Check the details before continuing.
</ui-alert>
```

## Attributes

These are boolean attributes: include the attribute to enable it and remove it to turn it off.

| Attribute | Effect |
| --- | --- |
| `selected` | Applies selected-state background and foreground colors. |
| `success` | Applies success-state colors. |
| `warning` | Applies warning-state colors. |
| `danger` | Applies danger-state colors. |
| `info` | Applies info-state colors. |
| `highlight` | Applies highlight-state colors. |
| `invert` | Swaps the alert's current background and foreground treatment. |
| `outline` | Displays a border using the configurable border style and color. |
| `small` | Uses small text, padding, icon size, gap, and corner radius. |
| `large` | Uses large text, padding, icon size, gap, and corner radius. |

With none of the status attributes, the alert uses default colors and normal size. The source does not define a dedicated `normal` attribute. Status and appearance attributes can be combined, for example `<ui-alert outline warning>`.

## Slots and layout

The component's template has a header row and a separate content area:

| Slot | Purpose |
| --- | --- |
| Default (unnamed) | Main message, shown in the header. |
| `icon` | Optional icon at the start of the header. |
| `extra` | Optional additional element at the end of the header, commonly an action link or button. |
| `close` | Optional close control at the end of the header. Clicking it removes the alert and emits `closed`. |
| `content` | Optional additional content below the header. |

Example with all parts:

```html
<ui-alert danger>
  <ui-icon-warning slot="icon"></ui-icon-warning>
  <strong>Action required:</strong> review this change.
  <ui-link-button slot="extra" href="/help">Help</ui-link-button>
  <ui-icon-button slot="close" aria-label="Close alert">
    <ui-icon-cross></ui-icon-cross>
  </ui-icon-button>
  <div slot="content">
    Additional information can go here.
    <ui-button danger invert>Review</ui-button>
  </div>
</ui-alert>
```

Use the exact slot names above: the implementation uses `content` for the lower area and `extra` for the right-side action. (Some help-page wording/code examples use inconsistent slot names.)

The header uses a flex layout; its message area grows to occupy available space, placing `extra` and `close` content toward the right. The default content remains in the header. Put content that should appear beneath the header in an element assigned to `slot="content"`.

## Closing an alert

The component does not render a close button automatically. Supply a slotted control using `slot="close"`. When the control is clicked, the alert removes itself from the DOM and dispatches a plain `closed` event on the alert element:

```js
const alert = document.querySelector("ui-alert");

alert.addEventListener("closed", () => {
  console.log("Alert closed");
});
```

The event is dispatched directly on the alert and does not bubble. Register the listener on the alert itself (or dispatch your own bubbling event from an application wrapper if delegation is needed).

## CSS customization

Set CSS custom properties on `<ui-alert>` or an ancestor, such as `:root`. The alert consumes these variables in its shadow styles.

### Colors

| Variable | Default / fallback |
| --- | --- |
| `--ui-alert-default-background` | `var(--ui-default-background, hsl(0 0% 90% / 1))` |
| `--ui-alert-default-foreground` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-alert-selected-background` | `var(--ui-selected-background, hsl(215 50% 90% / 1))` |
| `--ui-alert-selected-foreground` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-alert-success-background` | `var(--ui-success-background, hsl(104 50% 90% / 1))` |
| `--ui-alert-success-foreground` | `var(--ui-success-foreground, hsl(104 50% 30% / 1))` |
| `--ui-alert-warning-background` | `var(--ui-warning-background, hsl(45 50% 90% / 1))` |
| `--ui-alert-warning-foreground` | `var(--ui-warning-foreground, hsl(45 50% 30% / 1))` |
| `--ui-alert-danger-background` | `var(--ui-danger-background, hsl(0 50% 90% / 1))` |
| `--ui-alert-danger-foreground` | `var(--ui-danger-foreground, hsl(0 50% 30% / 1))` |
| `--ui-alert-info-background` | `var(--ui-info-background, hsl(190 50% 90% / 1))` |
| `--ui-alert-info-foreground` | `var(--ui-info-foreground, hsl(190 90% 30% / 1))` |
| `--ui-alert-highlight-background` | `var(--ui-highlight-background, hsl(302 50% 90% / 1))` |
| `--ui-alert-highlight-foreground` | `var(--ui-highlight-foreground, hsl(302 50% 30% / 1))` |
| `--ui-alert-background` | Current background; defaults to the default-state background. |
| `--ui-alert-foreground` | Current foreground; defaults to the default-state foreground. Also colors icons through `--ui-icon-primary-color`. |

Example:

```css
:root {
  --ui-alert-warning-background: #fff3cd;
  --ui-alert-warning-foreground: #664d03;
}
```

### Typography, spacing, and shape

The component uses small, normal (default), or large values based on the matching size attribute.

| Variable | Default / fallback |
| --- | --- |
| `--ui-alert-small-font-size` | `var(--ui-font-size-small, 12px)` |
| `--ui-alert-normal-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-alert-large-font-size` | `var(--ui-font-size-large, 24px)` |
| `--ui-alert-small-padding` | `8px` |
| `--ui-alert-normal-padding` | `12px` |
| `--ui-alert-large-padding` | `18px` |
| `--ui-alert-small-icon-size` | `16px` |
| `--ui-alert-normal-icon-size` | `24px` |
| `--ui-alert-large-icon-size` | `36px` |
| `--ui-alert-small-gap` | `8px` |
| `--ui-alert-normal-gap` | `12px` |
| `--ui-alert-large-gap` | `18px` |
| `--ui-alert-border-small-radius` | `var(--ui-border-radius-small, 3px)` |
| `--ui-alert-border-normal-radius` | `var(--ui-border-radius-normal, 4px)` |
| `--ui-alert-border-large-radius` | `var(--ui-border-radius-large, 6px)` |
| `--ui-alert-border-style` | `var(--ui-border-style, solid)` |
| `--ui-alert-border-width` | `var(--ui-border-width, 1px)` |
| `--ui-alert-border-color` | `var(--ui-alert-foreground)` |
| `--ui-alert-box-shadow` | `var(--ui-border-box-shadow, none)` |

The `icon` slot's slotted element receives the current icon size as both its height and width.

### Direct current-style variables

These variables override the current computed styles and can be set directly for one-off customization:

| Variable | Default / fallback |
| --- | --- |
| `--ui-alert-font-size` | `var(--ui-alert-normal-font-size, var(--ui-font-size-normal, 16px))` |
| `--ui-alert-padding` | `var(--ui-alert-normal-padding, 12px)` |
| `--ui-alert-icon-size` | `var(--ui-alert-normal-icon-size, 24px)` |
| `--ui-alert-border-radius` | `var(--ui-alert-border-normal-radius, var(--ui-border-radius-normal, 4px))` |
| `--ui-alert-gap` | `var(--ui-alert-normal-gap, 12px)` |

Example:

```html
<ui-alert
  style="
    --ui-alert-background: gold;
    --ui-alert-foreground: brown;
    --ui-alert-font-size: 1.25rem;
    --ui-alert-padding: 0.5rem 1rem;
    --ui-alert-icon-size: 2rem;
    --ui-alert-gap: 1rem;">
  <ui-icon-warning slot="icon"></ui-icon-warning>
  Custom-styled alert
</ui-alert>
```

`outline` uses the border style and color variables. Border width is set on the host and may be customized with `--ui-alert-border-width`. `--ui-alert-box-shadow` controls the shadow.

## Accessibility and behavior notes

- The component is a message container, not an interactive control; it does not add alert/live-region semantics or an ARIA role. Add appropriate semantics (for example `role="alert"` for urgent, immediately announced content or `role="status"` for non-urgent updates) where the use case requires them.
- Give icon-only close controls an accessible name, such as `aria-label="Close alert"`.
- Forced-colors and increased-contrast modes add a border.
- Undefined slotted web components are hidden until defined.
