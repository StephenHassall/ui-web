# `<ui-content-menu>`

A vertical, content-oriented menu container. It provides the menu background and foreground, stacks its children, and lets menu child components adapt their appearance to the content-menu context.

## Import

```js
import ".../ui-web/ui-content-menu.js";
```

The component is registered as `<ui-content-menu>`. It has an open shadow root containing a wrapper `<div>` with a default slot. Place menu components and other content directly inside it.

## Basic usage

```html
<ui-content-menu style="width: 12rem;">
  <ui-menu-header>CONTENTS</ui-menu-header>
  <ui-menu-link href="#overview">Overview</ui-menu-link>
  <ui-menu-link href="#details" selected>Details</ui-menu-link>
  <ui-menu-separator></ui-menu-separator>
  <ui-menu-button>More options</ui-menu-button>
</ui-content-menu>
```

The container uses a vertical flex layout. Its children can expand to the available width; set a width on the host or its layout context as appropriate. The content-menu-specific menu item styles use smaller text by default than side-menu or popup-menu items.

## Attributes and API

The component defines no attributes, properties, events, or interaction logic of its own. Menu item behavior—such as links, selection, disabled state, indentation, and icons—is provided by the child components.

The shared Menus usage examples show it with `<ui-menu-header>`, `<ui-menu-button>`, and `<ui-menu-separator>`. The standalone test also uses `<ui-menu-link>`. For child-specific attributes and behavior, consult those components' references.

## Styling

The container itself uses these inherited CSS custom properties:

| Variable | Default / fallback |
| --- | --- |
| `--ui-menu-content-background` | `var(--ui-background, hsl(0 0% 97% / 1))` |
| `--ui-menu-content-foreground` | `var(--ui-foreground, hsl(0 0% 18% / 1))` |

Example:

```css
:root {
  --ui-menu-content-background: #f4f7fb;
  --ui-menu-content-foreground: #202b3a;
}
```

Other variables documented on the shared Menus CSS page control child menu components, not the `<ui-content-menu>` host itself. Set them on the content-menu host or an ancestor so nested components can inherit them.

| Variable | Applies to | Default / fallback |
| --- | --- | --- |
| `--ui-menu-content-font-size` | Buttons and links | `var(--ui-font-size-small, 12px)` |
| `--ui-menu-content-font-weight` | Buttons and links | `var(--ui-font-weight-normal, normal)` |
| `--ui-menu-content-padding` | Buttons and links | `4px 8px` |
| `--ui-menu-content-icon-size` | Button/link icons | `16px` |
| `--ui-menu-content-gap` | Button/link icon-to-text gap | `8px` |
| `--ui-menu-content-bar-width` | Button/link leading bar | `2px` |
| `--ui-menu-content-bar-color` | Button/link leading bar | `var(--ui-midground, hsl(0 0% 90% / 1))` |
| `--ui-menu-selected-background` | Selected items | `var(--ui-selected-background, hsl(215 50% 90% / 1))` |
| `--ui-menu-selected-foreground` | Selected items | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-menu-selected-bar-color` | Selected item bar | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-menu-disabled-foreground` | Disabled items | Foreground/background mix |
| `--ui-menu-indent-width` | Indented items | `24px` |
| `--ui-menu-header-content-font-size` | Menu headers | `var(--ui-font-size-small, 12px)` |
| `--ui-menu-header-content-font-weight` | Menu headers | `var(--ui-font-weight-bold, 600)` |
| `--ui-menu-header-content-padding` | Menu headers | `4px 8px` |
| `--ui-menu-separator-content-padding` | Separators | `4px 0` |
| `--ui-menu-separator-content-bar-color` | Separators | `var(--ui-midground, hsl(0 0% 90% / 1))` |
| `--ui-menu-separator-content-bar-width` | Separators | `2px` |

The host can also be styled directly with ordinary CSS (for example `width`). It does not define internal padding, borders, or a gap between children. Undefined slotted custom elements are hidden until their definitions are loaded.

## Notes

- `<ui-content-menu>` is a layout and styling context; it does not implement a navigation model or manage which child is selected.
