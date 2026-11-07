# `<ui-icon-link-button>`

`<ui-icon-link-button>` is a link styled as a compact icon button. It contains a native anchor in its shadow root, so navigation and downloads use link behavior rather than button behavior.

## Import

```js
import ".../ui-web/ui-icon-link-button.js";
import ".../ui-web/icon/ui-icon-arrow-right.js";
```

Import the component and the icon elements you use. A custom `<svg>` or `<img>` can also be slotted as content.

## Basic usage

```html
<ui-icon-link-button href="/next" aria-label="Next page">
  <ui-icon-arrow-right></ui-icon-arrow-right>
</ui-icon-link-button>
```

The child content is rendered inside the native anchor. For an icon-only link, provide an accessible name on the link; the component does not add one automatically.

## Link attributes

The following anchor attributes are observed on the host and copied to the internal anchor when changed:

| Attribute | Purpose |
| --- | --- |
| `href` | Link destination. |
| `hreflang` | Language of the linked resource. |
| `download` | Indicates a download and optionally provides a filename. |
| `ping` | URLs notified when following the link. |
| `referrerpolicy` | Referrer information policy for the link. |
| `rel` | Relationship between the current page and destination. |
| `target` | Browsing context for the destination. |
| `type` | Advisory media type of the linked resource. |

Set `href` for a working link. The host exposes an `href` JavaScript property that reads the internal anchor's resolved URL, and a setter that updates or removes the host and internal anchor `href`.

## Disabled state

The `disabled` property reflects a host `disabled` attribute. The property setter also removes the internal anchor's `href` when disabling, and restores it from the host's `href` attribute when enabling:

```js
const link = document.querySelector("ui-icon-link-button");
link.disabled = true;
link.disabled = false;
```

The component also implements `formDisabledCallback()` and removes the anchor destination when disabled by a containing form context, such as a disabled fieldset. A disabled link is still an anchor-based component; it is not a native disabled button.

## Appearance attributes

| Attribute | Effect |
| --- | --- |
| `small` | Uses the small icon size, padding, and corner radius. |
| `large` | Uses the large icon size, padding, and corner radius. If both size attributes are present, large styles occur later and take precedence. |
| `circle` | Uses a circular shape and size-specific circle padding. |
| `selected` | Uses selected colors. |
| `success` | Uses success colors. |
| `warning` | Uses warning colors. |
| `danger` | Uses danger colors. |
| `info` | Uses info colors. |
| `highlight` | Uses highlight colors. |
| `invert` | Swaps foreground/background presentation. |
| `outline` | Uses a transparent background and border; the background fills during link hover or active interaction. |
| `blank` | Starts with a transparent background and shows its background on link hover or active interaction. |

The component host is `inline-block` and sized to fit. Slotted elements are sized to the configured square icon size.

## CSS customization

Set custom properties on `<ui-icon-link-button>` or an ancestor.

### Status colors

| Variable pair | Default / fallback |
| --- | --- |
| `--ui-icon-link-button-default-background` / `-foreground` | `var(--ui-default-background, hsl(0 0% 90% / 1))` / `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-icon-link-button-selected-background` / `-foreground` | `var(--ui-selected-background, hsl(215 50% 90% / 1))` / `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-icon-link-button-success-background` / `-foreground` | `var(--ui-success-background, hsl(104 50% 90% / 1))` / `var(--ui-success-foreground, hsl(104 50% 30% / 1))` |
| `--ui-icon-link-button-warning-background` / `-foreground` | `var(--ui-warning-background, hsl(45 50% 90% / 1))` / `var(--ui-warning-foreground, hsl(45 50% 30% / 1))` |
| `--ui-icon-link-button-danger-background` / `-foreground` | `var(--ui-danger-background, hsl(0 50% 90% / 1))` / `var(--ui-danger-foreground, hsl(0 50% 30% / 1))` |
| `--ui-icon-link-button-info-background` / `-foreground` | `var(--ui-info-background, hsl(190 50% 90% / 1))` / `var(--ui-info-foreground, hsl(190 90% 30% / 1))` |
| `--ui-icon-link-button-highlight-background` / `-foreground` | `var(--ui-highlight-background, hsl(302 50% 90% / 1))` / `var(--ui-highlight-foreground, hsl(302 50% 30% / 1))` |

Active presentation uses `--ui-icon-link-button-background` and `--ui-icon-link-button-foreground`, which default to the default palette and are switched by the semantic color attributes. The primary icon color is set to the active foreground for compatible icon artwork.

### Size, border, and shadow

| Variable | Normal default | Small / large defaults |
| --- | --- | --- |
| `--ui-icon-link-button-padding` | `6px` | `--ui-icon-link-button-small-padding: 4px`; `--ui-icon-link-button-large-padding: 8px` |
| `--ui-icon-link-button-circle-padding` | `8px` | `--ui-icon-link-button-small-circle-padding: 6px`; `--ui-icon-link-button-large-circle-padding: 10px` |
| `--ui-icon-link-button-icon-size` | `24px` | `--ui-icon-link-button-small-icon-size: 16px`; `--ui-icon-link-button-large-icon-size: 32px` |
| `--ui-icon-link-button-border-radius` | `var(--ui-border-radius-normal, 4px)` | Small: `var(--ui-border-radius-small, 3px)`; large: `var(--ui-border-radius-large, 6px)` |
| `--ui-icon-link-button-border-style` | `solid` | Used by `outline`. |
| `--ui-icon-link-button-border-width` | `var(--ui-border-width, 1px)` | Used by `outline`. |
| `--ui-icon-link-button-border-color` | `var(--ui-icon-link-button-foreground)` | Used by `outline`. |
| `--ui-icon-link-button-box-shadow` | `var(--ui-border-box-shadow, none)` | Applied to the anchor. |

The active values `--ui-icon-link-button-padding`, `--ui-icon-link-button-circle-padding`, `--ui-icon-link-button-icon-size`, and `--ui-icon-link-button-border-radius` can be set directly to override the size attributes.

```css
ui-icon-link-button {
  --ui-icon-link-button-default-background: #e0e7ff;
  --ui-icon-link-button-default-foreground: #3730a3;
  --ui-icon-link-button-icon-size: 28px;
  --ui-icon-link-button-padding: 8px;
}
```

The component provides a visible keyboard-focus outline. Forced-colors and increased-contrast modes add a stronger border.
