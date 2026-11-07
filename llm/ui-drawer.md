# `<ui-drawer>`

A drawer component that slides in from an edge of the page. It uses a native modal `<dialog>` element, so it blocks interaction with the page behind it while open.

## Import

```js
import UiDrawer from ".../ui-web/ui-drawer.js";
```

The default export is the `UiDrawer` class, which registers the `<ui-drawer>` custom element.

## Basic usage

```html
<ui-drawer name="settings-drawer" right>
  <section style="padding: 1rem; width: min(30rem, 80vi);">
    <h2>Settings</h2>
    <p>Drawer content goes here.</p>
  </section>
</ui-drawer>
```

```js
UiDrawer.open("settings-drawer");
UiDrawer.close("settings-drawer");
```

Each drawer needs a unique `name` attribute. Pass that same name to `open()` and `close()`. If `open(name)` is called before a matching drawer connects, it is scheduled to open when the matching element is connected.

## Static API

| Method | Behavior |
| --- | --- |
| `UiDrawer.open(name)` | Opens the connected drawer with the matching `name`. |
| `UiDrawer.close(name)` | Starts closing the connected drawer with the matching `name`. |
| `UiDrawer.getActive()` | Returns the class's active-drawer reference, or `null` if it is empty. |

The drawer is opened with the native `showModal()` method. Its closing animation runs before the native dialog is closed.

## Placement attributes

| Attribute | Effect |
| --- | --- |
| `left` | Places the drawer against the left edge. This is also the default when no edge attribute is present. |
| `right` | Places the drawer against the right edge. |
| `top` | Places the drawer against the top edge. |
| `bottom` | Places the drawer against the bottom edge. |

Use one placement attribute at a time. Left/right drawers size to their content, so constrain the content width to avoid filling the viewport. Top/bottom drawers size to their content height, so constrain or scroll tall content as needed.

## Closing behavior

- Clicking outside the drawer panel closes it.
- Pressing Escape closes it.
- `UiDrawer.close(name)` closes it programmatically.

The component listens for clicks on the native dialog and compares the click position with the drawer panel's bounds to detect outside clicks. Escape is handled through the native dialog's `cancel` event.

## CSS customization

Set these custom properties on `<ui-drawer>` or an ancestor:

| Variable | Default / fallback |
| --- | --- |
| `--ui-drawer-background` | `var(--ui-module-background, hsl(0 0% 97% / 1))` |
| `--ui-drawer-foreground` | `var(--ui-module-foreground, hsl(0 0% 18% / 1))` |
| `--ui-drawer-backdrop` | `var(--ui-module-backdrop, hsla(0, 0%, 97%, 0.25))` |
| `--ui-drawer-border-style` | `var(--ui-module-border-style, none)` |
| `--ui-drawer-border-width` | `var(--ui-module-border-width, 0)` |
| `--ui-drawer-border-color` | `var(--ui-module-border-color, transparent)` |
| `--ui-drawer-box-shadow` | `var(--ui-module-shadow, 0 1px 4px 0 rgb(from hsl(0 0% 18% / 1) r g b / 50%))` |

The border properties apply to the drawer edge facing the page: the inline end for left placement, inline start for right, block end for top, and block start for bottom. In forced-colors or increased-contrast modes, the component replaces the border with a two-pixel foreground border and removes the shadow. Reduced-motion preferences shorten the closing animation.

Example:

```css
ui-drawer {
  --ui-drawer-background: white;
  --ui-drawer-foreground: #222;
  --ui-drawer-backdrop: rgb(0 0 0 / 40%);
  --ui-drawer-border-style: solid;
  --ui-drawer-border-width: 1px;
  --ui-drawer-border-color: #bbb;
  --ui-drawer-box-shadow: 0 2px 12px rgb(0 0 0 / 20%);
}
```
