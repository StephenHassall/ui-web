# `<ui-button>`

A customizable button web component for actions, form submission/reset, and status or task emphasis.

## Import

```js
import ".../ui-web/ui-button.js";
```

The component is registered as `<ui-button>`. It uses an open shadow root and renders a native `<button type="button">` inside it. Put the button's label in the default slot. The component source imports `UiTools` from `../ui-tools.js` to register and load its HTML/CSS.

## Basic usage

```html
<ui-button>Save</ui-button>
<ui-button success>Save changes</ui-button>
<ui-button disabled>Unavailable</ui-button>
```

## Attributes

All options below are boolean attributes: include the attribute to enable it (for example, `disabled`); remove it to turn it off.

| Attribute | Effect |
| --- | --- |
| `disabled` | Disables the internal native button. Also exposed as the `disabled` property. |
| `one-click` | After a click, emits a click on the custom element and disables it to prevent another press. Re-enable it by removing `disabled` or setting `button.disabled = false`. Also exposed as the `oneClick` property. |
| `loading` | Shows an animated loading indicator over the button and hides its slotted content. Toggle this attribute to start/stop the indicator. Pair with `disabled` to prevent further presses while work is in progress. |
| `submit` | On click, calls `form.submit()` on the associated form, if there is one. |
| `reset` | On click, calls `form.reset()` on the associated form, if there is one. |
| `selected` | Applies selected-state colors. |
| `success` | Applies success-state colors. |
| `warning` | Applies warning-state colors. |
| `danger` | Applies danger-state colors. |
| `info` | Applies info-state colors. |
| `highlight` | Applies highlight-state colors. |
| `invert` | Swaps the current foreground and background treatment. |
| `outline` | Uses a transparent background and a configurable border; fills with the foreground color on hover/press. |
| `blank` | Removes the resting background; shows the background on hover/press. |
| `small` | Uses small text, spacing, icon size, and corner radius. |
| `large` | Uses large text, spacing, icon size, and corner radius. |
| `pill` | Gives the button pill-shaped corners and uses pill-specific padding. |
| `fill-width` | Makes the internal button take the host's available width and centers its contents. |

`submit` and `reset` are independent checks in the click handler; do not put both on the same button. The internal button defaults to `type="button"`, so a plain `<ui-button>` does not implicitly submit a form. For `submit`/`reset`, the component looks up its form through `ElementInternals` and does nothing if no form is associated. Its implementation calls `form.submit()` / `form.reset()` directly; `form.submit()` bypasses the normal submit-event and constraint-validation path.

The component is form-associated. Disabled state is also propagated to the internal native button when the containing form disables the custom element.

## Icons and content

Use the named `start` and `end` slots for optional icons. The default slot is the button body.

```html
<ui-button>
  <ui-icon-chevron-down slot="start"></ui-icon-chevron-down>
  Open menu
</ui-button>

<ui-button>
  Open in new tab
  <ui-icon-shortcut slot="end"></ui-icon-shortcut>
</ui-button>
```

`<ui-icon-*>` components, SVGs, images, and other slotted elements can be used as icons. Icon dimensions and padding are controlled by the icon CSS variables below. The body stays on one line and truncates overflowing text with an ellipsis.

## Common combinations

Appearance and size attributes can be combined, for example:

```html
<ui-button outline danger>Delete</ui-button>
<ui-button pill small info>Details</ui-button>
<ui-button loading disabled>Saving</ui-button>
<ui-button fill-width>Continue</ui-button>
```

Form controls:

```html
<form>
  <input name="user-name" placeholder="Enter your name">
  <ui-button submit success>Submit</ui-button>
  <ui-button reset outline>Reset</ui-button>
</form>
```

One-click action:

```html
<ui-button id="run-once" one-click>Run once</ui-button>
<button onclick="document.querySelector('#run-once').disabled = false">
  Enable again
</button>
```

## CSS customization

Set CSS custom properties on `<ui-button>` itself or on an ancestor (such as `:root`). The component's shadow styles consume these variables. State attributes select the state-specific colors; general variables can also be set directly to customize the current button.

### Colors

| Variable | Default / fallback |
| --- | --- |
| `--ui-button-default-background` | `var(--ui-default-background, hsl(0 0% 90% / 1))` |
| `--ui-button-default-foreground` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-button-selected-background` | `var(--ui-selected-background, hsl(215 50% 90% / 1))` |
| `--ui-button-selected-foreground` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-button-success-background` | `var(--ui-success-background, hsl(104 50% 90% / 1))` |
| `--ui-button-success-foreground` | `var(--ui-success-foreground, hsl(104 50% 30% / 1))` |
| `--ui-button-warning-background` | `var(--ui-warning-background, hsl(45 50% 90% / 1))` |
| `--ui-button-warning-foreground` | `var(--ui-warning-foreground, hsl(45 50% 30% / 1))` |
| `--ui-button-danger-background` | `var(--ui-danger-background, hsl(0 50% 90% / 1))` |
| `--ui-button-danger-foreground` | `var(--ui-danger-foreground, hsl(0 50% 30% / 1))` |
| `--ui-button-info-background` | `var(--ui-info-background, hsl(190 50% 90% / 1))` |
| `--ui-button-info-foreground` | `var(--ui-info-foreground, hsl(190 90% 30% / 1))` |
| `--ui-button-highlight-background` | `var(--ui-highlight-background, hsl(302 50% 90% / 1))` |
| `--ui-button-highlight-foreground` | `var(--ui-highlight-foreground, hsl(302 50% 30% / 1))` |
| `--ui-button-background` | Current default background; used by the regular button and as the basis for `invert`, `outline`, and `blank`. |
| `--ui-button-foreground` | Current default foreground; used by the regular button and as the basis for `invert`, `outline`, and `blank`. |

Example:

```css
:root {
  --ui-button-success-background: #d7f5c5;
  --ui-button-success-foreground: #174b0b;
}
```

### Typography, spacing, and shape

The size attributes select small, normal (the default), or large values. The listed defaults are the component fallbacks; theme-level variables can override them.

| Variable | Default / fallback |
| --- | --- |
| `--ui-button-small-font-size` | `var(--ui-font-size-small, 12px)` |
| `--ui-button-normal-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-button-large-font-size` | `var(--ui-font-size-large, 24px)` |
| `--ui-button-font-weight` | `var(--ui-font-weight-bold, 600)` |
| `--ui-button-small-padding` | `4px 8px` |
| `--ui-button-normal-padding` | `8px 16px` |
| `--ui-button-large-padding` | `12px 24px` |
| `--ui-button-small-pill-padding` | `4px 12px` |
| `--ui-button-normal-pill-padding` | `8px 12px` |
| `--ui-button-large-pill-padding` | `12px 24px` |
| `--ui-button-small-icon-padding` | `4px 0` |
| `--ui-button-normal-icon-padding` | `4px 0` |
| `--ui-button-large-icon-padding` | `4px 0` |
| `--ui-button-small-icon-size` | `12px` |
| `--ui-button-normal-icon-size` | `16px` |
| `--ui-button-large-icon-size` | `24px` |
| `--ui-button-small-gap` | `4px` |
| `--ui-button-normal-gap` | `8px` |
| `--ui-button-large-gap` | `12px` |
| `--ui-button-border-small-radius` | `var(--ui-border-radius-small, 3px)` |
| `--ui-button-border-normal-radius` | `var(--ui-border-radius-normal, 4px)` |
| `--ui-button-border-large-radius` | `var(--ui-border-radius-large, 6px)` |
| `--ui-button-border-style` | `solid` |
| `--ui-button-border-width` | `var(--ui-border-width, 1px)` |
| `--ui-button-border-color` | `var(--ui-button-foreground)` |
| `--ui-button-box-shadow` | `var(--ui-border-box-shadow, none)` |

### Direct current-style variables

These variables override the current computed style and are useful for one-off customization:

| Variable | Default / fallback |
| --- | --- |
| `--ui-button-font-size` | `var(--ui-button-normal-font-size, var(--ui-font-size-normal, 16px))` |
| `--ui-button-padding` | `var(--ui-button-normal-padding, 8px 16px)` |
| `--ui-button-pill-padding` | `var(--ui-button-normal-pill-padding, 8px 12px)` |
| `--ui-button-icon-padding` | `var(--ui-button-normal-icon-padding, 4px 0)` |
| `--ui-button-icon-size` | `var(--ui-button-normal-icon-size, 16px)` |
| `--ui-button-gap` | `var(--ui-button-normal-gap, 8px)` |
| `--ui-button-border-radius` | `var(--ui-button-border-normal-radius, var(--ui-border-radius-normal, 4px))` |

Example:

```html
<ui-button
  style="
    --ui-button-background: gold;
    --ui-button-foreground: brown;
    --ui-button-font-size: 1.25rem;
    --ui-button-padding: 0.5rem 1rem;
    --ui-button-gap: 0.5rem;">
  Custom style
</ui-button>
```

`outline` uses the border variables above. `--ui-button-box-shadow` controls the button shadow.

## Interaction and accessibility notes

- The internal element is a native button, preserving normal keyboard activation and disabled semantics.
- A visible keyboard focus receives an outline based on the current foreground color.
- Loading state uses a progress cursor and an animated indicator; reduced-motion preferences slow the spinner animation.
- Forced-colors and increased-contrast modes add a stronger border and focus outline.
- Slotted child elements do not receive pointer events; clicks on button content activate the button.
- `fill-width` stretches the internal button to the custom element's width; give the host or parent an available width when a full-width layout is desired.
