# `<ui-data-bar>`

`<ui-data-bar>` visualizes a value as a filled bar within a numeric range, with a text label. It is useful in table cells and supports positive-only, negative-only, and mixed ranges.

```js
import ".../ui-web/ui-data-bar.js";
```

Give it available width and height. The component's internal bar fills its host:

```html
<div style="width: 100%; height: 40px;">
  <ui-data-bar min="0" max="100" value="65" suffix="%"></ui-data-bar>
</div>
```

## Attributes

| Attribute | Default | Description |
|---|---:|---|
| `min` | `0` | Minimum of the displayed range. |
| `max` | `100` | Maximum of the displayed range. |
| `value` | `0` | Current numeric value. The bar is visually clamped to the range. |
| `prefix` | empty | Text before the value label. |
| `suffix` | empty | Text after the value label. |
| `text` | unset | Replaces the generated label; when present, `prefix` and `suffix` are ignored. |

These are observed HTML attributes, not JavaScript properties. Update them with `setAttribute()` or `removeAttribute()`:

```js
const bar = document.querySelector("ui-data-bar");
bar.setAttribute("value", "80");
bar.setAttribute("suffix", " units");
```

The generated label is `prefix + value attribute + suffix`. For example:

```html
<ui-data-bar min="0" max="100" value="42" prefix="$" suffix=" million"></ui-data-bar>
<ui-data-bar min="0" max="100" value="42" text="On target"></ui-data-bar>
```

The label uses the literal `value` attribute text; it is not automatically formatted with thousands separators or decimals.

## Range behavior

- For a range entirely above or at zero (`min >= 0`), the bar grows from the left.
- For a range entirely below or at zero (`max <= 0`), the bar grows from the right.
- For a range spanning zero, zero is the baseline: negative values fill left and non-negative values fill right.
- Values below `min` or above `max` are clamped for bar geometry. The label still shows the original `value` attribute.
- When the bar is too narrow for its label, the label is automatically positioned just outside the bar.
- A value at the range boundary can produce a full-length or zero-length bar.

## CSS custom properties

Set these properties on the component or an ancestor:

| Property | Default | Description |
|---|---|---|
| `--ui-data-bar-background` | `transparent` | Background of the unfilled track. |
| `--ui-data-bar-plus-color` | `#1858bb` | Bar color for non-negative values. |
| `--ui-data-bar-minus-color` | `#bb1818` | Bar color for negative values. |
| `--ui-data-bar-text-inside-color` | `hsl(0 0% 97% / 1)` | Label color while inside the bar. |
| `--ui-data-bar-text-outside-color` | `var(--ui-foreground, hsl(0 0% 18% / 1))` | Label color when moved outside the bar. |
| `--ui-data-bar-text-font-size` | `var(--ui-font-size-normal, 16px)` | Label font size and line height. |
| `--ui-data-bar-text-font-weight` | `var(--ui-font-weight-normal, normal)` | Label font weight. |
| `--ui-data-bar-text-gap` | `8px` | Space between the bar edge and label. |
| `--ui-data-bar-border-style` | `solid` | Bar border style. |
| `--ui-data-bar-border-width` | `0` | Bar border width. |
| `--ui-data-bar-border-color` | Darkened `--ui-data-bar-plus-color` | Bar border color. |
| `--ui-data-bar-border-raduis` | `var(--ui-border-radius-small, 3px)` | Bar corner radius. The property is spelled `raduis` in the implementation. |

Example:

```css
.metric {
  --ui-data-bar-plus-color: #26734d;
  --ui-data-bar-minus-color: #b42318;
  --ui-data-bar-text-font-weight: 600;
  --ui-data-bar-border-width: 1px;
}
```

## Implementation notes and limitations

- This is a visual indicator, not a native progress element. The implementation does not provide progress-bar ARIA semantics; provide an accessible textual value elsewhere when needed.
