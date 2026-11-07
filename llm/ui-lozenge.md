# `<ui-lozenge>`

`<ui-lozenge>` is a compact inline label for visually highlighting short text, such as a status, category, or emphasized term. It supports color variants, outlined and inverted appearances, optional icons, and three text sizes.

## Import

```js
import ".../ui-web/ui-lozenge.js";
```

## Basic usage

```html
<ui-lozenge info>Information</ui-lozenge>
<ui-lozenge success>Complete</ui-lozenge>
```

## Attributes

| Attribute | Effect |
| --- | --- |
| `selected`, `success`, `warning`, `danger`, `info`, `highlight` | Selects a corresponding background and foreground color pair. With multiple color attributes, later CSS rules determine the final pair (`highlight` has the highest precedence). |
| `invert` | Swaps the foreground and background colors. |
| `outline` | Uses a transparent background and a border in the foreground color. |
| `normal` | Uses the normal text size and its spacing. |
| `large` | Uses the large text size and its spacing. |

With neither `normal` nor `large`, the lozenge uses the small size. If both size attributes are present, the `large` styles take precedence.

## Content and icons

The default slot is the lozenge text. Text stays on one line and is truncated with an ellipsis when it exceeds the available width. The maximum width defaults to the host's available width and can be changed with `--ui-lozenge-max-width`.

Optional content can be placed in the `start` and `end` slots:

```html
<ui-lozenge warning>
  <ui-icon-warning slot="start"></ui-icon-warning>
  Check required
</ui-lozenge>
```

## CSS customization

Set these variables on `<ui-lozenge>` or an ancestor. Size-specific variables apply to `small` (the default), `normal`, or `large` variants.

| Variable | Default |
| --- | --- |
| `--ui-lozenge-default-background` / `--ui-lozenge-default-foreground` | Theme defaults; fallback `hsl(0 0% 90% / 1)` / `hsl(0 0% 30% / 1)` |
| `--ui-lozenge-selected-background` / `--ui-lozenge-selected-foreground` | Theme selected colors; fallback `hsl(215 50% 90% / 1)` / `hsl(215 50% 30% / 1)` |
| `--ui-lozenge-success-background` / `--ui-lozenge-success-foreground` | Theme success colors; fallback `hsl(104 50% 90% / 1)` / `hsl(104 50% 30% / 1)` |
| `--ui-lozenge-warning-background` / `--ui-lozenge-warning-foreground` | Theme warning colors; fallback `hsl(45 50% 90% / 1)` / `hsl(45 50% 30% / 1)` |
| `--ui-lozenge-danger-background` / `--ui-lozenge-danger-foreground` | Theme danger colors; fallback `hsl(0 50% 90% / 1)` / `hsl(0 50% 30% / 1)` |
| `--ui-lozenge-info-background` / `--ui-lozenge-info-foreground` | Theme info colors; fallback `hsl(190 50% 90% / 1)` / `hsl(190 90% 30% / 1)` |
| `--ui-lozenge-highlight-background` / `--ui-lozenge-highlight-foreground` | Theme highlight colors; fallback `hsl(302 50% 90% / 1)` / `hsl(302 50% 30% / 1)` |
| `--ui-lozenge-small-font-size` / `--ui-lozenge-normal-font-size` / `--ui-lozenge-large-font-size` | `12px` / `16px` / `24px` using shared font-size variables |
| `--ui-lozenge-font-weight` | `var(--ui-font-weight-bold, 600)` |
| `--ui-lozenge-small-padding` / `--ui-lozenge-normal-padding` / `--ui-lozenge-large-padding` | `0 4px` / `0 6px` / `0 8px` |
| `--ui-lozenge-small-icon-padding` / `--ui-lozenge-normal-icon-padding` / `--ui-lozenge-large-icon-padding` | `4px 0` |
| `--ui-lozenge-small-icon-size` / `--ui-lozenge-normal-icon-size` / `--ui-lozenge-large-icon-size` | `12px` / `16px` / `24px` |
| `--ui-lozenge-small-gap` / `--ui-lozenge-normal-gap` / `--ui-lozenge-large-gap` | `4px` / `6px` / `8px` |
| `--ui-lozenge-border-small-radius` / `--ui-lozenge-border-normal-radius` / `--ui-lozenge-border-large-radius` | `3px` / `4px` / `6px` using shared border-radius variables |
| `--ui-lozenge-border-style` | `solid` |
| `--ui-lozenge-border-width` | `var(--ui-border-width, 1px)` |
| `--ui-lozenge-border-color` | `var(--ui-lozenge-foreground)` |
| `--ui-lozenge-box-shadow` | `var(--ui-border-box-shadow, none)` |
| `--ui-lozenge-max-width` | `100%` |

The computed inline variables `--ui-lozenge-background`, `--ui-lozenge-foreground`, `--ui-lozenge-font-size`, `--ui-lozenge-padding`, `--ui-lozenge-icon-padding`, `--ui-lozenge-icon-size`, `--ui-lozenge-border-radius`, and `--ui-lozenge-gap` can also be overridden directly.

```css
ui-lozenge {
  --ui-lozenge-info-background: #dceeff;
  --ui-lozenge-info-foreground: #14365d;
  --ui-lozenge-normal-padding: 2px 8px;
  --ui-lozenge-max-width: 16rem;
}
```

