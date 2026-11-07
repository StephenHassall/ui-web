# `<ui-menu-link>`

`<ui-menu-link>` is a styled anchor for navigation items in popup, side, content, and header menus. It detects the nearest supported menu container and adopts its menu-specific typography, colors, spacing, icon sizing, and border styling.

## Import

```js
import ".../ui-web/ui-menu-link.js";
```

## Basic usage

```html
<ui-side-menu>
  <ui-menu-link href="/home">Home</ui-menu-link>
  <ui-menu-link href="/settings">Settings</ui-menu-link>
</ui-side-menu>
```

The link text is rendered in the default slot. Use `href` for navigation. The internal anchor supports the standard `hreflang`, `download`, `ping`, `referrerpolicy`, `rel`, `target`, and `type` attributes; these are forwarded from the host.

## Slots

Optional leading or trailing content can be assigned to `start` and `end`. Icons and other content such as lozenges are supported.

```html
<ui-menu-link href="/reports">
  <ui-icon-folder slot="start"></ui-icon-folder>
  Reports
  <ui-lozenge info slot="end">3</ui-lozenge>
</ui-menu-link>
```

## Attributes and properties

| Attribute | Effect |
| --- | --- |
| `href` | Sets the destination URL. |
| `hreflang`, `download`, `ping`, `referrerpolicy`, `rel`, `target`, `type` | Forwarded to the internal anchor. |
| `disabled` | Applies disabled colors and hover styling. When disabled by a containing form or fieldset, the component removes the internal anchor's `href`. |
| `selected` | Applies the selected menu colors and selection-bar styling. |
| `indent` | Adds indentation. An empty attribute means level `1`; numeric levels `1`–`20` are accepted. |

The `disabled` property reflects the host attribute. The `href` property returns the internal anchor's resolved URL. The component is form-associated to receive disabled-state callbacks; it does not expose a form value.

## Menu context and styling

The component adapts to its nearest `<ui-popup-menu>`, `<ui-side-menu>`, `<ui-content-menu>`, or `<ui-header-menu>`. Use the shared menu custom properties to style its context-specific colors, typography, spacing, borders, and icon sizes. See the shared menu reference for those variables.

Additional component-specific variables:

| Variable | Default | Effect |
| --- | --- | --- |
| `--ui-menu-link-hover-decoration` | `underline` | Decoration applied to the link text on hover. |
| `--ui-menu-indent-width` | `24px` | Base indentation distance for each level. |
| `--ui-menu-indent` | `1` | Internal multiplier set from the `indent` attribute. |

```css
ui-side-menu {
  --ui-menu-side-gap: 12px;
  --ui-menu-side-icon-size: 20px;
  --ui-menu-link-hover-decoration: underline;
  --ui-menu-indent-width: 20px;
}
```

The link uses an ellipsis for overflowing text. Keyboard focus is indicated with a visible outline. Hover and active feedback are applied when supported by the device.
