# `<ui-pattern>`

`<ui-pattern>` is a block-level web component that paints a repeating CSS background pattern. It can be used directly as a patterned region or nested inside another element as a patterned background layer. Its content is rendered through a default slot.

## Import

```js
import ".../ui-web/ui-pattern.js";
```

## Usage

Choose a pattern by adding one of its boolean attributes:

```html
<ui-pattern horizontal-bars></ui-pattern>
<ui-pattern vertical-bars></ui-pattern>
<ui-pattern triangles></ui-pattern>
<ui-pattern checker></ui-pattern>
<ui-pattern checker-angle></ui-pattern>
<ui-pattern cross></ui-pattern>
```

The component does not set its own width or height. Give it dimensions through CSS; its default display is `block`.

```html
<ui-pattern checker style="width: 12rem; height: 6rem;"></ui-pattern>
```

Pattern elements can also be placed inside a wrapper to compose or clip them:

```html
<ui-pattern style="display: flex; gap: 0.5rem;">
  <ui-pattern checker style="width: 5rem; height: 5rem;"></ui-pattern>
  <ui-pattern triangles style="width: 5rem; height: 5rem;"></ui-pattern>
</ui-pattern>
```

## Pattern attributes

| Attribute | Effect |
| --- | --- |
| `horizontal-bars` | Repeating two-color stripes. The implementation's gradient runs left to right, so the rendered stripes are vertical. |
| `vertical-bars` | Repeating two-color stripes. The implementation's gradient runs bottom to top, so the rendered stripes are horizontal. |
| `triangles` | Repeating diagonal two-color pattern. |
| `checker` | Repeating checkerboard pattern. |
| `checker-angle` | Repeating diagonally divided checker pattern. |
| `cross` | Repeating cross-like pattern. |

These are independent attributes, not mutually exclusive modes. If multiple pattern attributes are present, their CSS rules can override or combine background declarations according to stylesheet order; use one pattern attribute at a time for predictable results.

## Styling

Set `--ui-pattern-scale`, `--ui-pattern-color1`, and `--ui-pattern-color2` on the `<ui-pattern>` itself because the component defines host-level values for them. The base color and spread properties are inherited, so they can be set on the component or an ancestor.

| CSS custom property | Default | Description |
| --- | --- | --- |
| `--ui-pattern-scale` | `1` | Scales the repeating pattern tile. Most patterns use `0.5rem × scale`; `cross` and `checker-angle` use `1rem × scale`. |
| `--ui-pattern-color` | `var(--ui-midground, hsl(0 0% 90% / 1))` | Base color from which the two pattern colors are derived. |
| `--ui-pattern-color-spread` | `4` | Amount added to and subtracted from the base color's HSL lightness to derive the two colors. |
| `--ui-pattern-color1` | Derived from `--ui-pattern-color` and `--ui-pattern-color-spread` | First pattern color; set directly to override the derived color. |
| `--ui-pattern-color2` | Derived from `--ui-pattern-color` and `--ui-pattern-color-spread` | Second pattern color; set directly to override the derived color. |

```css
ui-pattern {
  --ui-pattern-scale: 2;
  --ui-pattern-color: lightblue;
  --ui-pattern-color-spread: 8;
}

ui-pattern.checker-custom {
  --ui-pattern-color1: navy;
  --ui-pattern-color2: lightblue;
}
```

The component uses relative HSL color calculations to create the derived colors. In forced-colors mode or when the user requests increased contrast, its background image is removed; `checker` retains its separately set background color.

## API

The component has no JavaScript properties, methods, or custom events. Its pattern choices are HTML attributes, and its content is exposed through the default slot.
