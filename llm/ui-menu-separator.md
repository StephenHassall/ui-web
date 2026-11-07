# `<ui-menu-separator>`

`<ui-menu-separator>` draws a divider between menu items. In popup, side, and content menus it is horizontal; in a header menu it is vertical.

## Import

```js
import ".../ui-web/ui-menu-separator.js";
```

## Usage

Add it between items in a supported menu. It has no attributes, JavaScript API, or slotted content.

```html
<ui-popup-menu>
  <ui-menu-button>Open</ui-menu-button>
  <ui-menu-separator></ui-menu-separator>
  <ui-menu-button>Settings</ui-menu-button>
</ui-popup-menu>

<ui-header-menu>
  <ui-menu-button>Home</ui-menu-button>
  <ui-menu-separator></ui-menu-separator>
  <ui-menu-button>Account</ui-menu-button>
</ui-header-menu>
```

The component detects its nearest `<ui-popup-menu>`, `<ui-side-menu>`, `<ui-content-menu>`, or `<ui-header-menu>` ancestor when connected. In a header menu, the divider uses a block-end border removed and an inline-end border instead.

## Styling

The separator's padding, divider color, and divider thickness can be customized independently for each menu context:

| Context | Padding variable (default) | Bar color variable (default) | Bar width variable (default) |
| --- | --- | --- | --- |
| Popup | `--ui-menu-separator-popup-padding` (`4px 0`) | `--ui-menu-separator-popup-bar-color` (`var(--ui-module-foreground, hsl(0 0% 18% / 1))`) | `--ui-menu-separator-popup-bar-width` (`1px`) |
| Side | `--ui-menu-separator-side-padding` (`4px 0`) | `--ui-menu-separator-side-bar-color` (`var(--ui-midground, hsl(0 0% 90% / 1))`) | `--ui-menu-separator-side-bar-width` (`2px`) |
| Content | `--ui-menu-separator-content-padding` (`4px 0`) | `--ui-menu-separator-content-bar-color` (`var(--ui-midground, hsl(0 0% 90% / 1))`) | `--ui-menu-separator-content-bar-width` (`2px`) |
| Header | `--ui-menu-separator-header-padding` (`0 4px`) | `--ui-menu-separator-header-bar-color` (`var(--ui-midground, hsl(0 0% 90% / 1))`) | `--ui-menu-separator-header-bar-width` (`2px`) |

```css
ui-side-menu {
  --ui-menu-separator-side-padding: 8px 12px;
  --ui-menu-separator-side-bar-color: #9ca3af;
  --ui-menu-separator-side-bar-width: 1px;
}
```

The component also uses the shared menu background and leading-bar variables. Outside a recognized menu, it has a horizontal divider with `2px` thickness, the shared midground color, `4px 0` padding, and the shared UI background.
