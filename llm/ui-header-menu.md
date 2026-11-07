# `<ui-header-menu>`

`<ui-header-menu>` is a horizontal menu container intended for page headers. It lays its slotted children out in a row and provides a background and foreground color. Add menu components such as `<ui-menu-header>`, `<ui-menu-button>`, and `<ui-menu-separator>` as children.

## Import

```js
import ".../ui-web/ui-header-menu.js";
```

Importing the module registers the `<ui-header-menu>` custom element. The element has no JavaScript API and no component-specific attributes.

Import the menu item components used inside the menu as well:

```js
import ".../ui-web/ui-menu-header.js";
import ".../ui-web/ui-menu-button.js";
import ".../ui-web/ui-menu-separator.js";
```

## Usage

```html
<ui-header-menu>
  <ui-menu-header>MY APP</ui-menu-header>
  <ui-menu-separator></ui-menu-separator>
  <ui-menu-button>Home</ui-menu-button>
  <ui-menu-button>Settings</ui-menu-button>
</ui-header-menu>
```

Children render through a default slot. The menu host uses `inline-flex`, lays children out in a row, centers them on the cross axis, and does not shrink. Its width is determined by its contents unless constrained by the surrounding layout or your own CSS.

## Styling

The host provides two component-level CSS custom properties. Set them directly on `<ui-header-menu>` or an ancestor:

| Variable | Default / fallback |
| --- | --- |
| `--ui-menu-header-background` | `var(--ui-module-background, hsl(0 0% 97% / 1))` |
| `--ui-menu-header-foreground` | `var(--ui-module-foreground, hsl(0 0% 18% / 1))` |

Example:

```css
ui-header-menu {
  --ui-menu-header-background: #172554;
  --ui-menu-header-foreground: white;
}
```

These variables style the menu container. Child components have their own styling; for example, `<ui-menu-header>` uses menu-type-specific variables for its text size, weight, and padding. See the references for those components when customizing menu items.

