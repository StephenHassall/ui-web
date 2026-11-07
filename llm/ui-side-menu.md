# `<ui-side-menu>`

`<ui-side-menu>` is a vertical menu container intended for navigation or lists of actions. It lays out its slotted children in a column and supplies the side-menu background and foreground colors used by menu child components.

## Import

```js
import ".../ui-web/ui-side-menu.js";
```

Import the menu item components used inside separately, such as `ui-menu-button`, `ui-menu-header`, `ui-menu-separator`, `ui-menu-group`, and `ui-popup`/`ui-popup-menu`.

## Usage

Set a width on the side menu so its contents have a defined width. Add menu components as children; they are placed in the default slot.

```html
<ui-side-menu style="width: 12rem;">
  <ui-menu-header>Settings</ui-menu-header>
  <ui-menu-button>General</ui-menu-button>
  <ui-menu-button selected>Appearance</ui-menu-button>
  <ui-menu-separator></ui-menu-separator>
  <ui-menu-button>About</ui-menu-button>
</ui-side-menu>
```

The side menu itself does not implement item selection, navigation, or menu state. Use the child components' attributes and behavior for those features. For example, `selected` is set on a menu item to style it as selected.
Slotted custom elements are hidden until their definitions are registered, so load the scripts for the child components you use.

### Submenus

Use `ui-popup` with a trigger and dialog slot to create a popup submenu within the side menu:

```html
<ui-side-menu style="width: 12rem;">
  <ui-menu-button>Home</ui-menu-button>
  <ui-popup right start>
    <ui-menu-button slot="trigger">
      More
      <ui-icon-chevron-right slot="end"></ui-icon-chevron-right>
    </ui-menu-button>
    <ui-popup-menu slot="dialog">
      <ui-menu-button>Submenu item</ui-menu-button>
    </ui-popup-menu>
  </ui-popup>
</ui-side-menu>
```

The side menu sets slotted `ui-popup` elements to `display: grid` so they participate correctly in the vertical layout.

## Attributes and API

`<ui-side-menu>` defines no component-specific attributes, properties, methods, or events. Set dimensions and other layout behavior with normal CSS. Its implementation is a simple container with an open shadow root and a default slot.

## CSS customization

Set these variables on `<ui-side-menu>` or an ancestor. They style the container and the child menu items when those items are inside the side menu.

| Variable | Default |
| --- | --- |
| `--ui-menu-side-background` | `var(--ui-background, hsl(0 0% 97% / 1))` |
| `--ui-menu-side-foreground` | `var(--ui-foreground, hsl(0 0% 18% / 1))` |
| `--ui-menu-side-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-menu-side-font-weight` | `var(--ui-font-weight-normal, normal)` |
| `--ui-menu-side-bar-color` | `transparent` |
| `--ui-menu-side-bar-width` | `4px` |
| `--ui-menu-selected-background` | `var(--ui-selected-background, hsl(215 50% 90% / 1))` |
| `--ui-menu-selected-foreground` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-menu-disabled-foreground` | Mix of standard foreground and background colors, each at 50% in `srgb-linear` |
| `--ui-menu-indent-width` | `24px` |
| `--ui-menu-side-padding` | `4px 8px` |
| `--ui-menu-side-icon-size` | `16px` |
| `--ui-menu-side-gap` | `8px` |
| `--ui-menu-group-side-arrow-size` | `32px` |

`--ui-menu-side-*` variables affect the side-menu appearance. The selected, disabled, and indent variables are shared by multiple menu components. The arrow-size variable applies to `ui-menu-group` children.
