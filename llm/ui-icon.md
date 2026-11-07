# `<ui-icon>` base class

`UiIcon` is the shared base class used by the library's SVG icon custom elements. It creates an open shadow root, inserts the SVG supplied by the derived class, and applies common sizing, color, and right-to-left direction behavior. It is not the SVG artwork for every icon; each concrete icon has its own custom element and SVG definition.

## Import and use an icon

Import the JavaScript module for each concrete icon you use. Importing an icon module imports the base class and registers that icon's custom element.

```js
import ".../ui-web/icon/ui-icon-ring.js";
import ".../ui-web/icon/ui-icon-bar-chart.js";
```

```html
<ui-icon-ring style="height: 1.5rem;"></ui-icon-ring>
<ui-icon-bar-chart
  style="height: 2rem; --ui-icon-primary-color: rebeccapurple;">
</ui-icon-bar-chart>
```

Concrete icon classes extend `UiIcon` and pass their SVG markup to its constructor. The base class is intended to be reused by those icon components rather than used as a standalone, fully drawn icon. Icons can also be placed in component slots, such as `slot="start"` or `slot="end"` on buttons.

## Sizing

The base SVG is `display: block`, has an aspect ratio of `1 / 1`, fills its available height, and has minimum width and height of `8px`. Set the icon's height to control its rendered size; the SVG keeps a square aspect ratio.

```html
<ui-icon-ring style="height: 24px;"></ui-icon-ring>
```

## Colors

The base class defines two SVG-level color aliases:

| Variable | Default |
| --- | --- |
| `--ui-c1` | `var(--ui-icon-primary-color, var(--ui-foreground, hsl(0 0% 18% / 1)))` |
| `--ui-c2` | `var(--ui-icon-secondary-color, var(--ui-c1))` |

Set `--ui-icon-primary-color` and, where supported by the icon artwork, `--ui-icon-secondary-color` on the icon or an ancestor. Individual SVG definitions decide which paths use these aliases or the public color variables, so secondary-color behavior can vary between icon designs.

```css
ui-icon-ring {
  --ui-icon-primary-color: rebeccapurple;
}
```

## Right-to-left direction

Add `match-direction` to mirror the SVG horizontally when it is in a right-to-left (`rtl`) direction context. This is useful for directional artwork when the visual direction should follow the text direction.

```html
<ui-icon-arrow-bar-right match-direction></ui-icon-arrow-bar-right>
```

Without `match-direction`, the SVG keeps its authored orientation. The base class does not automatically mirror all icons.
