# `<ui-link-button>`

`<ui-link-button>` styles a native link to look like a button. It is intended for navigation and downloads, not for actions that should use a button control.

## Import

```js
import ".../ui-web/ui-link-button.js";
```

## Basic usage

```html
<ui-link-button href="/account">View account</ui-link-button>
```

The component renders a native `<a>` in its shadow DOM. Without `href`, the anchor is not a navigable link. Use a native button component for actions that do not navigate.

## Link attributes

The component observes these host attributes and forwards them to the inner anchor:

| Attribute | Effect |
| --- | --- |
| `href` | Link destination. |
| `target` | Browsing context, such as `_blank`. |
| `rel` | Relationship to the destination; use `rel="noopener"` with `_blank` when appropriate. |
| `hreflang` | Language of the linked resource. |
| `download` | Requests that the linked resource be downloaded. |
| `ping` | URLs notified by the browser when the link is followed. |
| `referrerpolicy` | Referrer information policy for the link. |
| `type` | MIME type hint for the linked resource. |

## Appearance attributes

| Attribute | Effect |
| --- | --- |
| `selected` | Uses selected-state colors. |
| `success`, `warning`, `danger`, `info`, `highlight` | Uses the corresponding status color pair. |
| `invert` | Swaps the foreground and background colors. |
| `outline` | Uses a transparent background and a colored border; the background fills on hover/focus. |
| `blank` | Uses a transparent background until hover/focus. |
| `disabled` | Intended to disable navigation and apply a disabled appearance. |
| `small`, `large` | Selects the small or large size; the default is normal. |
| `pill` | Uses a fully rounded shape and pill-specific padding. |
| `fill-width` | Makes the inner anchor fill the host's available width. |

The states and appearance attributes are visual and may be combined. If multiple color-state attributes are present, the CSS rules' order determines which color pair wins.

## JavaScript properties

The component exposes `href` and `disabled` properties:

```js
const link = document.querySelector("ui-link-button");
link.href = "/account";
link.disabled = true;
```

`href` reads the inner anchor's resolved URL. Setting `disabled` to `true` removes the inner anchor's `href`; setting it to `false` restores the host's `href`.

## Content and icons

Default slot content is the link text. The text is kept on one line and truncated with an ellipsis when it does not fit. Optional icons or other content can be placed in the `start` and `end` slots:

```html
<ui-link-button href="/settings">
  <ui-icon-settings slot="start"></ui-icon-settings>
  Settings
</ui-link-button>
```

## CSS customization

Set these variables on `<ui-link-button>` or an ancestor. Size-specific variables affect the corresponding `small`, default/normal, or `large` size.

| Variable | Default |
| --- | --- |
| `--ui-link-button-default-background` | `var(--ui-default-background, hsl(0 0% 90% / 1))` |
| `--ui-link-button-default-foreground` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-link-button-selected-background` | `var(--ui-selected-background, hsl(215 50% 90% / 1))` |
| `--ui-link-button-selected-foreground` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-link-button-success-background` / `-foreground` | Theme success colors; fallback `hsl(104 50% 90% / 1)` / `hsl(104 50% 30% / 1)` |
| `--ui-link-button-warning-background` / `-foreground` | Theme warning colors; fallback `hsl(45 50% 90% / 1)` / `hsl(45 50% 30% / 1)` |
| `--ui-link-button-danger-background` / `-foreground` | Theme danger colors; fallback `hsl(0 50% 90% / 1)` / `hsl(0 50% 30% / 1)` |
| `--ui-link-button-info-background` / `-foreground` | Theme info colors; fallback `hsl(190 50% 90% / 1)` / `hsl(190 90% 30% / 1)` |
| `--ui-link-button-highlight-background` / `-foreground` | Theme highlight colors; fallback `hsl(302 50% 90% / 1)` / `hsl(302 50% 30% / 1)` |
| `--ui-link-button-small-font-size` / `--ui-link-button-normal-font-size` / `--ui-link-button-large-font-size` | `12px` / `16px` / `24px` using shared font-size variables |
| `--ui-link-button-font-weight` | `var(--ui-font-weight-bold, 600)` |
| `--ui-link-button-small-padding` / `--ui-link-button-normal-padding` / `--ui-link-button-large-padding` | `4px 8px` / `8px 16px` / `12px 24px` |
| `--ui-link-button-small-pill-padding` / `--ui-link-button-normal-pill-padding` / `--ui-link-button-large-pill-padding` | `4px 12px` / `8px 12px` / `12px 24px` |
| `--ui-link-button-small-icon-size` / `--ui-link-button-normal-icon-size` / `--ui-link-button-large-icon-size` | `12px` / `16px` / `24px` |
| `--ui-link-button-small-icon-padding` / `--ui-link-button-normal-icon-padding` / `--ui-link-button-large-icon-padding` | `4px 0` |
| `--ui-link-button-small-gap` / `--ui-link-button-normal-gap` / `--ui-link-button-large-gap` | `4px` / `8px` / `12px` |
| `--ui-link-button-border-small-radius` / `--ui-link-button-border-normal-radius` / `--ui-link-button-border-large-radius` | `3px` / `4px` / `6px` using shared border-radius variables |
| `--ui-link-button-border-style` | `solid` |
| `--ui-link-button-border-width` | `var(--ui-border-width, 1px)` |
| `--ui-link-button-border-color` | `var(--ui-link-button-foreground)` |
| `--ui-link-button-box-shadow` | `var(--ui-border-box-shadow, none)` |

Inline variables can override the computed default colors and sizing:

```css
ui-link-button {
  --ui-link-button-default-background: rebeccapurple;
  --ui-link-button-default-foreground: white;
  --ui-link-button-normal-padding: 10px 20px;
  --ui-link-button-normal-gap: 12px;
}
```
