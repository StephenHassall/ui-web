# Shared CSS defaults

The shared CSS custom properties give the web components a common theme for typography, colors, borders, animation, input controls, menus, and charts. The source catalogs `src/chart.css`, `src/inputs.css`, and `src/menu.css` are marked “for information only”; they document the shared variables and defaults rather than applying styles directly. Component styles may use these documented values as fallbacks. The theme stylesheet loaded by the application provides the global theme tokens.

Set a variable on `:root` to theme the page, or on a containing element/component to scope the override:

```css
:root {
  --ui-background: #fff;
  --ui-foreground: #202124;
  --ui-chart-color1: #1769aa;
}

.settings-panel {
  --ui-input-border-radius: 8px;
}
```

Many component-specific variables reference shared tokens and also include a literal fallback. Overriding a shared token affects all consumers that use it; setting a component-specific variable changes only that component's corresponding style. Individual component guides list their own variables.

## Standard theme tokens

`web-dev/ui/api-standard-css.html` documents these shared defaults. The default values are also in `src/theme/light.css`, `src/theme/dark.css`, and `src/theme/system.css`.

### Typography, borders, and animation

| Variables | Defaults |
|---|---|
| `--ui-font-size-small`, `--ui-font-size-normal`, `--ui-font-size-large` | `12px`, `16px`, `24px` |
| `--ui-font-weight-normal`, `--ui-font-weight-bold` | `normal`, `600` |
| `--ui-border-radius-small`, `--ui-border-radius-normal`, `--ui-border-radius-large` | `3px`, `4px`, `6px` |
| `--ui-border-width`, `--ui-border-style` | `1px`, `solid` |
| `--ui-animation-duration-slow`, `--ui-animation-duration-default`, `--ui-animation-duration-quick` | `400ms`, `200ms`, `100ms` |
| `--ui-border-box-shadow` | `none` in the system theme |

Components may use a subset of these tokens or a component-specific override. For example, an input's border width is controlled by `--ui-input-border-width`, not necessarily the shared `--ui-border-width`.

### Theme colors

The system and light themes use the light values; the dark theme and system theme's dark color-scheme media query use the dark values. Semantic pairs are listed as background / foreground:

| Tokens | Light value(s) | Dark value(s) |
|---|---|---|
| `--ui-background` | `hsl(0 0% 97% / 1)` | `hsl(0 0% 18% / 1)` |
| `--ui-midground` | `hsl(0 0% 90% / 1)` | `hsl(0 0% 25% / 1)` |
| `--ui-foreground` | `hsl(0 0% 18% / 1)` | `hsl(0 0% 97% / 1)` |
| `--ui-default-background` / `--ui-default-foreground` | `hsl(0 0% 90% / 1)` / `hsl(0 0% 30% / 1)` | `hsl(0 0% 30% / 1)` / `hsl(0 0% 90% / 1)` |
| `--ui-selected-background` / `--ui-selected-foreground` | `hsl(215 50% 90% / 1)` / `hsl(215 50% 30% / 1)` | `hsl(215 50% 30% / 1)` / `hsl(215 50% 90% / 1)` |
| `--ui-success-background` / `--ui-success-foreground` | `hsl(104 50% 90% / 1)` / `hsl(104 50% 30% / 1)` | `hsl(104 50% 30% / 1)` / `hsl(104 50% 90% / 1)` |
| `--ui-warning-background` / `--ui-warning-foreground` | `hsl(45 50% 90% / 1)` / `hsl(45 50% 30% / 1)` | `hsl(45 50% 30% / 1)` / `hsl(45 50% 90% / 1)` |
| `--ui-danger-background` / `--ui-danger-foreground` | `hsl(0 50% 90% / 1)` / `hsl(0 50% 30% / 1)` | `hsl(0 50% 30% / 1)` / `hsl(0 50% 90% / 1)` |
| `--ui-info-background` / `--ui-info-foreground` | `hsl(190 50% 90% / 1)` / `hsl(190 90% 30% / 1)` | `hsl(190 90% 30% / 1)` / `hsl(190 50% 90% / 1)` |
| `--ui-highlight-background` / `--ui-highlight-foreground` | `hsl(302 50% 90% / 1)` / `hsl(302 50% 30% / 1)` | `hsl(302 50% 30% / 1)` / `hsl(302 50% 90% / 1)` |

### Overlay/module tokens

Dialogs, drawers, and popup-style components can use the shared module variables:

| Variable | Light/system-light default | Dark/system-dark default |
|---|---|---|
| `--ui-module-background` | `hsl(0 0% 97% / 1)` | `hsl(0 0% 25% / 1)` in `system.css`; `hsl(0 0% 18% / 1)` in `dark.css` |
| `--ui-module-foreground` | `hsl(0 0% 18% / 1)` | `hsl(0 0% 75% / 1)` |
| `--ui-module-backdrop` | `hsla(0, 0%, 97%, 0.25)` | `hsla(0, 0%, 25%, 0.25)` |
| `--ui-module-border-style`, `--ui-module-border-width`, `--ui-module-border-color` | `none`, `0`, `transparent` | `none`, `0`, `transparent` |
| `--ui-module-shadow` | `0 1px 4px 0 rgb(from var(--ui-module-foreground, hsl(0 0% 18% / 1)) r g b / 50%)` | System: `0 0 2px 0 var(--ui-module-foreground)`; dark: `0 0 3px 0 var(--ui-module-foreground)` |

The system theme uses a slightly lighter module surface (`hsl(0 0% 25% / 1)`) and smaller shadow in dark mode than the fixed dark theme. Consult the selected theme file when exact appearance matters.

## Chart palette

`src/chart.css` documents a shared 20-color palette. Components can use `--ui-chart-color1` through `--ui-chart-color20`; override the tokens to recolor charts consistently:

| Variable | Default | Variable | Default |
|---|---|---|---|
| `--ui-chart-color1` | `#1858bb` | `--ui-chart-color11` | `#757575` |
| `--ui-chart-color2` | `#109b95` | `--ui-chart-color12` | `#c0ca33` |
| `--ui-chart-color3` | `#64c056` | `--ui-chart-color13` | `#c35a09` |
| `--ui-chart-color4` | `#e1b038` | `--ui-chart-color14` | `#9678df` |
| `--ui-chart-color5` | `#ed4e30` | `--ui-chart-color15` | `#b71c54` |
| `--ui-chart-color6` | `#df66b0` | `--ui-chart-color16` | `#2d8bd5` |
| `--ui-chart-color7` | `#756bb2` | `--ui-chart-color17` | `#67d0dd` |
| `--ui-chart-color8` | `#18b3bb` | `--ui-chart-color18` | `#607d8b` |
| `--ui-chart-color9` | `#a32dab` | `--ui-chart-color19` | `#b0bec5` |
| `--ui-chart-color10` | `#b78675` | `--ui-chart-color20` | `#347936` |

Not every chart uses the same fallback sequence when a token is unset; explicit colors or these shared variables are the reliable way to keep charts consistent. See the relevant chart guide for component-specific color behavior.

## Shared input defaults

`src/inputs.css` documents the variables used by input components. Component-specific input guides already describe these shared defaults where applicable; this table provides the common catalog.

| Variable | Default |
|---|---|
| `--ui-input-background` | `field` |
| `--ui-input-foreground` | `fieldtext` |
| `--ui-input-placeholder` | `color-mix(in hsl, var(--ui-input-background, field) 70%, var(--ui-input-foreground, fieldText) 30%)` |
| `--ui-input-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-input-font-weight` | `var(--ui-font-weight-normal, normal)` |
| `--ui-input-padding` | `8px 8px` |
| `--ui-input-border-radius` | `var(--ui-border-radius-normal, 4px)` |
| `--ui-input-border-style` | `var(--ui-border-style, solid)` |
| `--ui-input-border-width` | `2px` |
| `--ui-input-border-color` | `var(--ui-midground, hsl(0 0% 90% / 1))` |
| `--ui-input-border-valid-color`, `--ui-input-border-invalid-color` | `hsl(120 80% 50% / 1)`, `hsl(0 80% 50% / 1)` |
| `--ui-input-outline-style`, `--ui-input-outline-width`, `--ui-input-outline-offset`, `--ui-input-outline-color` | `none`, `0`, `0`, `transparent` |
| `--ui-input-outline-valid-color`, `--ui-input-outline-invalid-color` | `transparent`, `transparent` |
| `--ui-input-focus-width`, `--ui-input-focus-shadow-width` | `2px`, `4px` |
| `--ui-input-focus-color` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-input-focus-shadow-color` | `var(--ui-selected-background, hsl(215 50% 90% / 1))` |
| `--ui-input-focus-valid-color`, `--ui-input-focus-valid-shadow-color` | `hsl(120 80% 50% / 1)`, `hsl(120 50% 80% / 1)` |
| `--ui-input-focus-invalid-color`, `--ui-input-focus-invalid-shadow-color` | `hsl(0 80% 50% / 1)`, `hsl(0 50% 80% / 1)` |
| `--ui-input-disabled-background` | `color-mix(in srgb-linear, var(--ui-foreground, hsl(0 0% 18% / 1)) 75%, var(--ui-background, hsl(0 0% 97% / 1)) 25%)` |
| `--ui-input-disabled-foreground` | `color-mix(in srgb-linear, var(--ui-foreground, hsl(0 0% 18% / 1)) 25%, var(--ui-background, hsl(0 0% 97% / 1)) 75%)` |
| `--ui-input-box-shadow` | `var(--ui-border-box-shadow, none)` |
| `--ui-input-icon-valid-color`, `--ui-input-icon-invalid-color` | `hsl(120 80% 50% / 1)`, `hsl(0 80% 50% / 1)` |
| `--ui-input-icon-size`, `--ui-input-icon-padding`, `--ui-input-gap` | `16px`, `0`, `8px` |

`field` and `fieldtext` are browser system colors, so they adapt to the platform's native form-control appearance. Valid/invalid state colors are fixed green/red defaults unless overridden.

## Shared menu defaults

`src/menu.css` has defaults for popup, side, content, and header menu presentations. Component-specific menu guides cover the subset each component consumes.

| Variable group | Defaults |
|---|---|
| Background | `--ui-menu-popup-background: var(--ui-module-background, hsl(0 0% 97% / 1))`; side/content/header backgrounds use `var(--ui-background, hsl(0 0% 97% / 1))`. |
| Foreground | Popup uses `var(--ui-module-foreground, hsl(0 0% 18% / 1))`; side/content/header use `var(--ui-foreground, hsl(0 0% 18% / 1))`. |
| Selected colors | `--ui-menu-selected-background: var(--ui-selected-background, hsl(215 50% 90% / 1))`; `--ui-menu-selected-foreground: var(--ui-selected-foreground, hsl(215 50% 30% / 1))`. |
| Font size | Popup/side/header: `var(--ui-font-size-normal, 16px)`; content: `var(--ui-font-size-small, 12px)`. |
| Font weight | Popup/side/content/header: `var(--ui-font-weight-normal, normal)`. |
| Bar colors | Popup and side: `transparent`; content: `var(--ui-midground, hsl(0 0% 90% / 1))`; selected: `var(--ui-selected-foreground, hsl(215 50% 30% / 1))`. |
| Bar widths | Popup `0`; side `4px`; content `2px`. |
| Indentation | `--ui-menu-indent-width: 24px`. |
| Disabled foreground | 50/50 `color-mix()` of shared foreground and background. |
| Padding | Popup/side/content/header: `4px 8px`. |
| Icon size | Popup/side/content/header: `16px`. |
| Gap | Popup/side/content/header: `8px`. |

Menu popup colors use module tokens by default, while side/content/header menus use page background and foreground tokens. This distinction keeps floating menus visually grouped with dialogs and other overlays.

## Accessibility and theme behavior

- The light and dark theme files share font sizes, border radii, and animation durations; their color values differ.
- `system.css` responds to `prefers-color-scheme`. Its reduced-motion rule sets the three shared animation-duration tokens to `0`.
- The light, dark, and system themes also include high-contrast/forced-color adjustments. In those modes they set stronger border styling, remove module shadows, and map semantic colors back to the main background/foreground pair.
- Variables listed here are defaults, not a requirement to use them. Prefer semantic shared tokens (such as `--ui-background`, `--ui-foreground`, and `--ui-selected-*`) over hard-coded colors when creating a theme.
