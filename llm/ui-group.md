# `<ui-group>`

`<ui-group>` arranges its slotted child elements as a bordered group. By default, children are stacked vertically. Add `inline` to lay them out horizontally, and add `bar` to draw separators between adjacent children.

## Import

```js
import ".../ui-web/ui-group.js";
```

Importing the module registers the `<ui-group>` custom element. The component has no JavaScript API; its behavior is controlled with attributes and CSS.

## Basic usage

```html
<ui-group bar>
  <div style="padding: 0.5rem;">First item</div>
  <div style="padding: 0.5rem;">Second item</div>
  <div style="padding: 0.5rem;">Third item</div>
</ui-group>
```

```html
<ui-group inline bar>
  <ui-button>Cancel</ui-button>
  <ui-button>Save</ui-button>
</ui-group>
```

Children are rendered through a default slot. The group itself does not add padding; provide padding on the child elements when needed. The component sets child margins to `0 !important`.

## Attributes

| Attribute | Effect |
| --- | --- |
| `inline` | Arranges children in a row instead of a column. |
| `bar` | Adds separators between adjacent children. In a vertical group these are horizontal borders; in an inline group they are vertical borders. |

Without `inline`, the first child gets the top and side borders, the last child gets the bottom and side borders, and middle children get side borders. With `inline`, the first child gets the left and top/bottom borders, the last gets the right and top/bottom borders, and middle children get top/bottom borders. A single child receives a border on all sides. `bar` adds the corresponding separator border to every child except the last.

The borders are applied to slotted children, not to the `<ui-group>` host. Consequently, slotted elements' border styles can be affected by the group, and the border color, width, and radius apply to their edges.

## CSS customization

Set these custom properties on `<ui-group>` or an ancestor:

| Variable | Default / fallback |
| --- | --- |
| `--ui-group-border-radius` | `var(--ui-border-radius-large, 6px)` |
| `--ui-group-border-style` | `var(--ui-border-style, solid)` |
| `--ui-group-border-width` | `var(--ui-border-width, 1px)` |
| `--ui-group-border-color` | `var(--ui-midground, hsl(0 0% 90% / 1))` |

Example:

```html
<ui-group
  bar
  style="
    --ui-group-border-radius: 1rem;
    --ui-group-border-style: dashed;
    --ui-group-border-width: 2px;
    --ui-group-border-color: rebeccapurple;">
  <div style="padding: 0.5rem;">First item</div>
  <div style="padding: 0.5rem;">Second item</div>
</ui-group>
```
