# `<ui-menu-group>`

`<ui-menu-group>` is an expandable parent item for popup, side, and content menus. It displays a main item with a chevron and reveals child menu items when expanded. The usage help advises against using it in `<ui-header-menu>`.

## Import

```js
import ".../ui-web/ui-menu-group.js";
```

## Basic usage

Place the parent item's text and optional `start`/`end` content directly inside the component. Put its child items in an element assigned to the `group` slot:

```html
<ui-side-menu>
  <ui-menu-group>
    Products
    <div slot="group">
      <ui-menu-button indent>Product A</ui-menu-button>
      <ui-menu-button indent>Product B</ui-menu-button>
    </div>
  </ui-menu-group>
</ui-side-menu>
```

Child items are not automatically indented; apply `indent` to them as needed.

## Attributes

| Attribute | Effect |
| --- | --- |
| `open` | Shows the group slot. The group starts closed if this attribute is absent. Clicking the chevron toggles it. |
| `arrow-button` | Gives the chevron its own interactive hover/active appearance. This is useful when the main item has an `href`. |
| `arrow-left` | Places the chevron on the left side. |
| `arrow-hide` | Hides the chevron when there is no `href`. Use only when the main item can still be clicked to expand/collapse. |
| `href` | Turns the main item into a link and forwards the standard link attributes to its internal anchor. Use with `arrow-button` so the arrow can still toggle the child group. |
| `disabled` | Applies disabled styling to the main item. When used with a link, it is intended to prevent navigation while leaving the arrow available to expand/collapse children. |
| `selected` | Applies selected menu colors and indicator styling. |
| `indent` | Indents the main item using the shared `--ui-menu-indent-width` and `--ui-menu-indent` variables. |

The internal anchor also receives standard link attributes including `target`, `rel`, `hreflang`, `download`, `ping`, `referrerpolicy`, and `type`.

Clicking the main item without `href` toggles the group via its chevron. With `href`, the main item navigates and the arrow button controls expansion separately.

The component also exposes a `disabled` property that reads or toggles the host's `disabled` attribute, and an `href` getter that returns the inner anchor's resolved URL.

## Content and icons

The default slot is the parent item's label. The `start` and `end` slots accept icons or other supplementary content. The child group must use the named `group` slot.

```html
<ui-menu-group arrow-button>
  <ui-icon-folder slot="start"></ui-icon-folder>
  Documents
  <ui-lozenge info slot="end">3</ui-lozenge>
  <div slot="group">
    <ui-menu-button indent>Reports</ui-menu-button>
  </div>
</ui-menu-group>
```

## CSS customization

The component uses shared menu variables for menu-specific background, foreground, typography, padding, icon sizes, gaps, and selection bars. It also exposes:

| Variable | Default |
| --- | --- |
| `--ui-menu-group-popup-arrow-size` | `32px` |
| `--ui-menu-group-side-arrow-size` | `32px` |
| `--ui-menu-group-content-arrow-size` | `32px` |
| `--ui-menu-group-link-hover-decoration` | `underline` |

Its direction is adjusted for `arrow-left` and right-to-left layout.

```css
ui-side-menu {
  --ui-menu-group-side-arrow-size: 24px;
  --ui-menu-group-link-hover-decoration: underline;
  --ui-menu-selected-background: #dceeff;
  --ui-menu-indent-width: 20px;
}
```

See the shared menus CSS reference for the menu variables used by this component.
