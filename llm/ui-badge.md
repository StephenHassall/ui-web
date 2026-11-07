# `<ui-badge>`

A compact, pill-shaped label for highlighting a short value, such as a count, status, or numeric metric. The component accepts arbitrary slotted content; it does not parse or manage a numeric value.

## Import

```js
import ".../ui-web/ui-badge.js";
```

The component is registered as `<ui-badge>`. It renders its contents through the default slot in an open shadow root.

## Basic usage

```html
<ui-badge>3</ui-badge>
<ui-badge success>Ready</ui-badge>
<ui-badge danger>-5</ui-badge>
```

By default, the badge uses the small text size and small padding. It is an inline-block element, centers its text, prevents wrapping, and has rounded pill-shaped ends.

## Attributes

These are boolean attributes: include the attribute to enable it and remove it to turn it off.

| Attribute | Effect |
| --- | --- |
| `selected` | Applies selected-state background and foreground colors. |
| `success` | Applies success-state colors. |
| `warning` | Applies warning-state colors. |
| `danger` | Applies danger-state colors. |
| `info` | Applies info-state colors. |
| `highlight` | Applies highlight-state colors. |
| `invert` | Swaps the current background and foreground treatment. |
| `outline` | Uses a transparent background and a configurable border. |
| `normal` | Uses normal text size and padding. |
| `large` | Uses large text size and padding. |

With no size attribute, the badge is small. There is no `small` attribute; small is the default. The `normal` attribute selects normal sizing. Color and appearance attributes can be combined, for example `<ui-badge outline danger>!</ui-badge>`.

## CSS customization

Set CSS custom properties on `<ui-badge>` or an ancestor, such as `:root`. The badge consumes these variables in its shadow styles.

### Colors

| Variable | Default / fallback |
| --- | --- |
| `--ui-badge-default-background` | `var(--ui-default-background, hsl(0 0% 90% / 1))` |
| `--ui-badge-default-foreground` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-badge-selected-background` | `var(--ui-selected-background, hsl(215 50% 90% / 1))` |
| `--ui-badge-selected-foreground` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-badge-success-background` | `var(--ui-success-background, hsl(104 50% 90% / 1))` |
| `--ui-badge-success-foreground` | `var(--ui-success-foreground, hsl(104 50% 30% / 1))` |
| `--ui-badge-warning-background` | `var(--ui-warning-background, hsl(45 50% 90% / 1))` |
| `--ui-badge-warning-foreground` | `var(--ui-warning-foreground, hsl(45 50% 30% / 1))` |
| `--ui-badge-danger-background` | `var(--ui-danger-background, hsl(0 50% 90% / 1))` |
| `--ui-badge-danger-foreground` | `var(--ui-danger-foreground, hsl(0 50% 30% / 1))` |
| `--ui-badge-info-background` | `var(--ui-info-background, hsl(190 50% 90% / 1))` |
| `--ui-badge-info-foreground` | `var(--ui-info-foreground, hsl(190 90% 30% / 1))` |
| `--ui-badge-highlight-background` | `var(--ui-highlight-background, hsl(302 50% 90% / 1))` |
| `--ui-badge-highlight-foreground` | `var(--ui-highlight-foreground, hsl(302 50% 30% / 1))` |
| `--ui-badge-background` | Current background; defaults to the default-state background. |
| `--ui-badge-foreground` | Current foreground; defaults to the default-state foreground. |

Example:

```css
:root {
  --ui-badge-success-background: #d7f5c5;
  --ui-badge-success-foreground: #174b0b;
}
```

### Typography, padding, and borders

| Variable | Default / fallback |
| --- | --- |
| `--ui-badge-small-font-size` | `var(--ui-font-size-small, 12px)` |
| `--ui-badge-normal-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-badge-large-font-size` | `var(--ui-font-size-large, 24px)` |
| `--ui-badge-small-padding` | `0 4px` |
| `--ui-badge-normal-padding` | `0 6px` |
| `--ui-badge-large-padding` | `0 8px` |
| `--ui-badge-border-style` | `var(--ui-border-style, solid)` |
| `--ui-badge-border-width` | `var(--ui-border-width, 1px)` |
| `--ui-badge-border-color` | `var(--ui-badge-foreground)` |
| `--ui-badge-box-shadow` | `var(--ui-border-box-shadow, none)` |

`outline` uses the border style, width, and color variables. The border width defaults to the shared `--ui-border-width` (or `1px`).

### Direct current-style variables

These variables override the current computed style and are useful for one-off customization:

| Variable | Default / fallback |
| --- | --- |
| `--ui-badge-font-size` | `var(--ui-badge-small-font-size, var(--ui-font-size-small, 12px))` |
| `--ui-badge-padding` | `var(--ui-badge-small-padding, 0 4px)` |

Example:

```html
<ui-badge
  style="
    --ui-badge-background: gold;
    --ui-badge-foreground: brown;
    --ui-badge-font-size: 1.25rem;
    --ui-badge-padding: 0.25rem 0.75rem;">
  42
</ui-badge>
```

The badge has a fixed pill-like border radius (`10rem`) and does not expose a custom property for changing its radius.

## Behavior and accessibility notes

- The component is presentational: it has no built-in interaction, properties, or events.
- It does not add a role or accessible semantics. Add appropriate semantics in the surrounding markup if the value conveys status or changes dynamically.
- The default style prevents line wrapping and centers text. Its minimum width is based on the current font size.
- Forced-colors and increased-contrast modes add a border.
