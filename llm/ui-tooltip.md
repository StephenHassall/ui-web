# `<ui-tooltip>`

`<ui-tooltip>` wraps a trigger element and displays a popup message while the pointer hovers over the component. Import the component module:

```js
import ".../ui-web/ui-tooltip.js";
```

```html
<ui-tooltip>
  <button>Help</button>
  <span slot="message">More information about this action.</span>
</ui-tooltip>
```

## Content and behavior

- Put the trigger element or other visible content in the default (unnamed) slot.
- Put the tooltip content in the `message` slot. The message slot can contain formatted HTML.
- The message is positioned above the trigger by default.
- The message appears on hover only on devices that support hover. It has a 0.5-second delay before its 0.3-second fade-in.
- On devices without hover support, the message is hidden.
- The component has no JavaScript behavior or public properties/events; its display and position are provided by CSS.

## Position attributes

Add one of these boolean attributes to position the message relative to the trigger:

| Attribute | Position |
|---|---|
| (none) or `top` | Above, horizontally centered (default). |
| `bottom` | Below, horizontally centered. |
| `left` | To the left, vertically centered. |
| `right` | To the right, vertically centered. |

Use only one position attribute at a time.

```html
<ui-tooltip right>
  <button>Help</button>
  <span slot="message">Message appears to the right.</span>
</ui-tooltip>
```

## CSS customization

Set these custom properties on the component or an ancestor:

| Property | Default / purpose |
|---|---|
| `--ui-tooltip-background` | Popup background; falls back to `--ui-foreground`, then `hsl(0 0% 18% / 1)`. |
| `--ui-tooltip-foreground` | Popup text color; falls back to `--ui-background`, then `hsl(0 0% 90% / 1)`. |
| `--ui-tooltip-font-size` | Message font size; falls back to `--ui-font-size-small`, then `12px`. |
| `--ui-tooltip-font-weight` | Message font weight; falls back to `--ui-font-weight-normal`, then `normal`. |
| `--ui-tooltip-padding` | Message inner padding; `4px 6px`. |
| `--ui-tooltip-radius` | Message corner radius; falls back to `--ui-border-radius-small`, then `3px`. |
| `--ui-tooltip-gap` | Space between the trigger and message; `8px`. |

```css
ui-tooltip {
  --ui-tooltip-background: #222;
  --ui-tooltip-foreground: white;
  --ui-tooltip-gap: 12px;
}
```

## Accessibility and limitations

The component does not add tooltip roles or ARIA relationships, and it does not reveal the message on keyboard focus. Do not rely on this tooltip as the only way to provide essential information; make important instructions available through an accessible description or visible text as well. The message uses `white-space: nowrap`, so it does not wrap automatically.
