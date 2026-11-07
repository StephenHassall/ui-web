# `<ui-menu-button>`

`<ui-menu-button>` is a button used as an item in popup, side, content, and header menus. In a popup menu, clicking an ordinary menu item closes the outermost popup unless `keep-open` is set.

## Import

```js
import ".../ui-web/ui-menu-button.js";
```

## Basic usage

```html
<ui-popup-menu>
  <ui-menu-button>Open</ui-menu-button>
  <ui-menu-button selected>Current page</ui-menu-button>
  <ui-menu-button disabled>Unavailable</ui-menu-button>
</ui-popup-menu>
```

The component renders a native `<button type="button">` and uses the surrounding menu type to select its styling. It does not submit a form.

## Attributes

| Attribute | Effect |
| --- | --- |
| `selected` | Applies selected menu colors and selected indicator styling. |
| `disabled` | Intended to mark the item unavailable. The native button is disabled when disabled by a containing form or fieldset. |
| `indent` | Adds indentation at the start. With no value, the indent level is `1`; numeric levels from `1` through `20` are supported by the component's style calculation. |
| `keep-open` | Prevents a click from closing the containing popup menu. |
| `slot="trigger"` | When placed inside a `ui-popup`, clicking the menu button acts as a popup trigger and does not close an ancestor popup. Use it with the popup's trigger slot behavior. |

When a clicked menu item is not a trigger, its click handler finds the outermost ancestor `<ui-popup>` and calls `close()`. If there is no popup ancestor, it does not close a menu.

## Content and icons

The default slot is the menu item's text. Text stays on one line and is truncated with an ellipsis when it exceeds the available width. Supplementary content can be placed in `start` and `end` slots:

```html
<ui-menu-button>
  <ui-icon-settings slot="start"></ui-icon-settings>
  Settings
  <ui-lozenge info slot="end">NEW</ui-lozenge>
</ui-menu-button>
```

## Styling

The component adapts to its nearest containing menu type and uses the corresponding shared menu CSS variables. Common variables include:

| Purpose | Variables |
| --- | --- |
| Menu background and foreground | `--ui-menu-popup-background` / `-foreground`, `--ui-menu-side-background` / `-foreground`, `--ui-menu-content-background` / `-foreground`, `--ui-menu-header-background` / `-foreground` |
| Selected state | `--ui-menu-selected-background`, `--ui-menu-selected-foreground`, `--ui-menu-selected-bar-color` |
| Disabled text and icons | `--ui-menu-disabled-foreground` |
| Indentation | `--ui-menu-indent-width` (default `24px`) |
| Menu-specific padding, icon size, and gaps | `--ui-menu-{popup,side,content,header}-padding`, `--ui-menu-{popup,side,content,header}-icon-size`, `--ui-menu-{popup,side,content,header}-gap` |

Additional menu-specific variables control typography and side/content selection bars. See the shared menus CSS reference for the complete variable list.

```css
ui-side-menu {
  --ui-menu-side-background: #f7f7f7;
  --ui-menu-side-foreground: #222;
  --ui-menu-selected-background: #dceeff;
  --ui-menu-indent-width: 20px;
}
```

