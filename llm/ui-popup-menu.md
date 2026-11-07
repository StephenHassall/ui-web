# `<ui-popup-menu>`

`<ui-popup-menu>` is a vertically arranged menu container, typically used as the dialog content of `<ui-popup>`. Add menu items, headers, separators, and nested popups as its children.

## Import

```js
import ".../ui-web/ui-popup-menu.js";
```

## Usage

```html
<ui-popup bottom>
  <ui-button slot="trigger">Open menu</ui-button>
  <ui-popup-menu slot="dialog">
    <ui-menu-header>Actions</ui-menu-header>
    <ui-menu-button>Edit</ui-menu-button>
    <ui-menu-button>Duplicate</ui-menu-button>
    <ui-menu-separator></ui-menu-separator>
    <ui-menu-button>Delete</ui-menu-button>
  </ui-popup-menu>
</ui-popup>
```

The menu is designed to work with `<ui-popup>`, which supplies the trigger and popup behavior. It can contain nested `<ui-popup>` elements to create submenus. Menu items and related components adapt their appearance to the surrounding menu type.

## Content

The component exposes a default slot for all menu contents. Common children include:

- `<ui-menu-header>` for a menu heading.
- `<ui-menu-button>` for an action or selectable item.
- `<ui-menu-link>` for a navigational item.
- `<ui-menu-separator>` for a divider.
- `<ui-popup>` with another `<ui-popup-menu slot="dialog">` for a submenu.

The menu width is determined by its contents. Set a width on `<ui-popup-menu>` if a specific width is required.

## Styling

The menu uses a vertical flex layout and centers its children along the vertical axis. It has a rounded background, a shadow, and top and bottom padding based on `--ui-border-radius-large` (default `6px`).

| CSS custom property | Default |
| --- | --- |
| `--ui-menu-popup-background` | `var(--ui-module-background, hsl(0 0% 97% / 1))` |
| `--ui-menu-popup-foreground` | `var(--ui-module-foreground, hsl(0 0% 18% / 1))` |
| `--ui-border-radius-large` | `6px` fallback |

Set the shared menu variables on the menu or an ancestor. Menu items within it also use shared variables for typography, padding, icon sizes, gaps, selection colors, and disabled colors; see the shared menus CSS reference for the complete set.

```css
ui-popup-menu {
  --ui-menu-popup-background: #fff;
  --ui-menu-popup-foreground: #222;
  --ui-menu-popup-font-size: 1rem;
}
```

In forced-colors mode or when increased contrast is requested, the menu removes its shadow and padding and adds a solid border using the shared border variables.

## API

`<ui-popup-menu>` has no JavaScript properties, methods, or custom events. Its content is supplied through the default slot.
