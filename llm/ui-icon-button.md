# `<ui-icon-button>`

`<ui-icon-button>` is a compact button intended to display an icon. It wraps a native `button` in a shadow root and renders its children in the button. Use a concise accessible label for the action; the component does not add an accessible name automatically.

## Import

```js
import ".../ui-web/ui-icon-button.js";
import ".../ui-web/icon/ui-icon-plus.js";
```

Import the component and the icon elements you use. The button can also contain your own `<svg>` or `<img>`.

## Basic usage

```html
<ui-icon-button aria-label="Add item">
  <ui-icon-plus></ui-icon-plus>
</ui-icon-button>
```

The internal native button has `type="button"`. Use attributes to select its appearance and behavior:

```html
<ui-icon-button
  aria-label="Save"
  success
  outline
  large>
  <ui-icon-check></ui-icon-check>
</ui-icon-button>
```

## Attributes and properties

| Attribute | Effect |
| --- | --- |
| `small` | Uses the small icon, padding, and corner-radius settings. |
| `large` | Uses the large icon, padding, and corner-radius settings. If both `small` and `large` are present, the large rules appear later in the stylesheet and take precedence. |
| `circle` | Gives the button a circular shape and uses the circle padding settings. |
| `selected` | Applies selected colors. |
| `success` | Applies success colors. |
| `warning` | Applies warning colors. |
| `danger` | Applies danger colors. |
| `info` | Applies info colors. |
| `highlight` | Applies highlight colors. |
| `invert` | Swaps foreground/background presentation. |
| `outline` | Uses a transparent background and a configurable border; the background fills on hover or active interaction. |
| `blank` | Starts with a transparent background and displays its background color on hover or active interaction. |
| `loading` | Replaces the slotted content visually with an animated indicator. It does not automatically disable the button. |
| `one-click` | After a click, invokes a click on the host and sets the host's `disabled` attribute. |
| `disabled` | Exposes a `disabled` Boolean property that reflects the host attribute. The current implementation does not synchronize this host attribute to the internal button; see the note below. |
| `submit` | On click, calls `submit()` on the associated form, if there is one. |
| `reset` | On click, calls `reset()` on the associated form, if there is one. |

`disabled` and `oneClick` are also JavaScript properties:

```js
const button = document.querySelector("ui-icon-button");
button.disabled = true;
button.oneClick = true;
```

The component is form-associated and uses `ElementInternals` to find its form. The internal button itself is `type="button"`; `submit` and `reset` are explicit host attributes that invoke the form methods. The component does not define a form value or a `name`/`value` submission API.

When a containing form disables the custom element (for example, through a disabled fieldset), `formDisabledCallback()` applies `disabled` to its internal native button.

## Child content and sizing

Child elements are placed in the button's default slot. Slotted content is sized to the configured icon size. The defaults are 24px for a normal button, 16px for `small`, and 32px for `large`. The host is `inline-block` and sized to fit its content.

```html
<ui-icon-button aria-label="Search">
  <img src="/icons/search.svg" alt="">
</ui-icon-button>
```

The component does not infer a text alternative from an icon. Supply an accessible name, such as `aria-label`, for icon-only actions.

## CSS customization

Set custom properties on `<ui-icon-button>` or an ancestor. Status color properties determine the semantic palette; the active state color properties can also be overridden directly.

### Status colors

| Variable | Default / fallback |
| --- | --- |
| `--ui-icon-button-default-background` | `var(--ui-default-background, hsl(0 0% 90% / 1))` |
| `--ui-icon-button-default-foreground` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-icon-button-selected-background` | `var(--ui-selected-background, hsl(215 50% 90% / 1))` |
| `--ui-icon-button-selected-foreground` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-icon-button-success-background` | `var(--ui-success-background, hsl(104 50% 90% / 1))` |
| `--ui-icon-button-success-foreground` | `var(--ui-success-foreground, hsl(104 50% 30% / 1))` |
| `--ui-icon-button-warning-background` | `var(--ui-warning-background, hsl(45 50% 90% / 1))` |
| `--ui-icon-button-warning-foreground` | `var(--ui-warning-foreground, hsl(45 50% 30% / 1))` |
| `--ui-icon-button-danger-background` | `var(--ui-danger-background, hsl(0 50% 90% / 1))` |
| `--ui-icon-button-danger-foreground` | `var(--ui-danger-foreground, hsl(0 50% 30% / 1))` |
| `--ui-icon-button-info-background` | `var(--ui-info-background, hsl(190 50% 90% / 1))` |
| `--ui-icon-button-info-foreground` | `var(--ui-info-foreground, hsl(190 90% 30% / 1))` |
| `--ui-icon-button-highlight-background` | `var(--ui-highlight-background, hsl(302 50% 90% / 1))` |
| `--ui-icon-button-highlight-foreground` | `var(--ui-highlight-foreground, hsl(302 50% 30% / 1))` |

The active values are `--ui-icon-button-background` and `--ui-icon-button-foreground`. They default to the default palette and are switched by the semantic attributes. The button also sets `--ui-icon-primary-color` to its foreground color for compatible icon artwork.

### Size, border, and shadow

| Variable | Normal default | Small / large defaults |
| --- | --- | --- |
| `--ui-icon-button-padding` | `6px` | `--ui-icon-button-small-padding: 4px`; `--ui-icon-button-large-padding: 8px` |
| `--ui-icon-button-circle-padding` | `8px` | `--ui-icon-button-small-circle-padding: 6px`; `--ui-icon-button-large-circle-padding: 10px` |
| `--ui-icon-button-icon-size` | `24px` | `--ui-icon-button-small-icon-size: 16px`; `--ui-icon-button-large-icon-size: 32px` |
| `--ui-icon-button-border-radius` | `var(--ui-border-radius-normal, 4px)` | Small: `var(--ui-border-radius-small, 3px)`; large: `var(--ui-border-radius-large, 6px)` |
| `--ui-icon-button-border-style` | `solid` | Used by `outline`. |
| `--ui-icon-button-border-width` | `var(--ui-border-width, 1px)` | Used by `outline`. |
| `--ui-icon-button-border-color` | `var(--ui-icon-button-foreground)` | Used by `outline`. |
| `--ui-icon-button-box-shadow` | `var(--ui-border-box-shadow, none)` | Applied to the native button. |

The active layout values `--ui-icon-button-padding`, `--ui-icon-button-circle-padding`, `--ui-icon-button-icon-size`, and `--ui-icon-button-border-radius` can be set directly to override the size attributes. `circle` uses the circle padding variable.

```css
ui-icon-button {
  --ui-icon-button-default-background: #e0e7ff;
  --ui-icon-button-default-foreground: #3730a3;
  --ui-icon-button-icon-size: 28px;
  --ui-icon-button-padding: 8px;
}
```

The component provides a visible keyboard-focus outline. Forced-colors and increased-contrast modes add a stronger border. Reduced-motion mode slows the loading spinner.

