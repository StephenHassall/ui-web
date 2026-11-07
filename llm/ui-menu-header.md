# `<ui-menu-header>`

`<ui-menu-header>` displays a title or section label in a menu. Its typography, padding, background, and leading border adapt to the nearest supported menu container: `<ui-popup-menu>`, `<ui-side-menu>`, `<ui-content-menu>`, or `<ui-header-menu>`.

## Import

```js
import ".../ui-web/ui-menu-header.js";
```

## Usage

Place the component among the menu's other children. Its text or other content goes in the default slot.

```html
<ui-side-menu>
  <ui-menu-header>ACCOUNT</ui-menu-header>
  <ui-menu-button>Profile</ui-menu-button>
  <ui-menu-button>Preferences</ui-menu-button>
  <ui-menu-header>SUPPORT</ui-menu-header>
  <ui-menu-button>Help</ui-menu-button>
</ui-side-menu>
```

The component has no attributes or JavaScript API. It detects the nearest supported menu ancestor when connected to the document and selects the corresponding styles.

## Styling

Outside a recognized menu, the component uses inherited font family, normal 16px text, normal weight, 150% line height, the shared background and foreground colors, and `4px 8px` padding.

Within a recognized menu, these custom properties control the header's font size, weight, and padding. Set them on the menu or an ancestor:

| Menu context | Font-size variable (default) | Font-weight variable (default) | Padding variable (default) |
| --- | --- | --- | --- |
| Popup | `--ui-menu-header-popup-font-size` (`var(--ui-font-size-small, 12px)`) | `--ui-menu-header-popup-font-weight` (`var(--ui-font-weight-bold, 600)`) | `--ui-menu-header-popup-padding` (`4px 8px`) |
| Side | `--ui-menu-header-side-font-size` (`var(--ui-font-size-small, 12px)`) | `--ui-menu-header-side-font-weight` (`var(--ui-font-weight-bold, 600)`) | `--ui-menu-header-side-padding` (`4px 8px`) |
| Content | `--ui-menu-header-content-font-size` (`var(--ui-font-size-small, 12px)`) | `--ui-menu-header-content-font-weight` (`var(--ui-font-weight-bold, 600)`) | `--ui-menu-header-content-padding` (`4px 8px`) |
| Header | `--ui-menu-header-header-font-size` (`var(--ui-font-size-small, 12px)`) | `--ui-menu-header-header-font-weight` (`var(--ui-font-weight-bold, 600)`) | `--ui-menu-header-header-padding` (`4px 8px`) |

```css
ui-side-menu {
  --ui-menu-header-side-font-size: 14px;
  --ui-menu-header-side-font-weight: 700;
  --ui-menu-header-side-padding: 6px 12px;
}
```

The component also uses shared menu variables for context-specific background and leading border:

| Context | Background variable | Leading border |
| --- | --- | --- |
| Popup | `--ui-menu-popup-background` | `--ui-menu-popup-bar-width` and `--ui-menu-popup-bar-color` |
| Side | `--ui-menu-side-background` | `--ui-menu-side-bar-width` and `--ui-menu-side-bar-color` |
| Content | `--ui-menu-content-background` | `--ui-menu-content-bar-width` and `--ui-menu-content-bar-color` |
| Header | `--ui-menu-header-background` | No leading border |

Backgrounds fall back to the shared menu or UI background variables. Popup and side leading-bar colors default to transparent; the content-menu bar defaults to the shared midground color. In header menus, the component uses a full-height grid layout with centered content.

The component is presentational: it renders a `div` containing a default slot, not a native heading element.
