# `<ui-tab>`

`<ui-tab>` presents a set of selectable tabs and their corresponding panels. Tabs and panels are connected by their order: the first tab controls the first panel, the second controls the second, and so on.

## Import

```js
import ".../ui-web/ui-tab.js";
```

## Basic usage

Put each tab in `slot="tab"` and each panel in `slot="panel"`. The component numbers them from 1, in DOM order.

```html
<ui-tab>
  <button slot="tab">Overview</button>
  <button slot="tab">Details</button>

  <section slot="panel">Overview content</section>
  <section slot="panel">Details content</section>
</ui-tab>
```

The tab and panel counts should match when panels are used. Panels are optional; tabs can be used without them. The component does not validate that the counts match.

Use the `selected` attribute on one tab to choose the initial selection:

```html
<ui-tab>
  <div slot="tab">First</div>
  <div slot="tab" selected>Second</div>
  <div slot="panel">First panel</div>
  <div slot="panel">Second panel</div>
</ui-tab>
```

If no tab has `selected`, the first tab is selected when panel slot assignment initializes. If multiple tabs have `selected`, the first one in DOM order wins. The component sets and removes `selected` as the user or program selects tabs.

## Attributes

| Attribute | Effect |
| --- | --- |
| `selected` | Put on a slotted tab to request the initial selection. The component manages this attribute after initialization. |
| `boxed` | Uses the boxed tab and panel appearance. |
| `bar` | Shows separator bars between tabs in addition to the selected-tab indicator. |
| `top` | Places tabs at the top. This is also the default position. |
| `bottom` | Places tabs below the panel. |
| `left` | Places tabs vertically on the left. |
| `right` | Places tabs vertically on the right. |

Use at most one position attribute. The stylesheet gives `left` precedence over `right`, and either vertical position takes precedence over `bottom`; `top` is equivalent to the default position.

## Tab content and slots

| Slot / marker | Purpose |
| --- | --- |
| `tab` | Required for each selectable tab. Use a simple element with text for the built-in label style, or a button component for button styling. |
| `panel` | Optional content corresponding by order to each tab. Only the current panel is shown. |
| `ui-tab-button` | Add this attribute to a `slot="tab"` element to style it as a button rather than a text label. Useful for custom content such as a lozenge. |

`<ui-button>`, `<ui-link-button>`, and `<ui-icon-button>` used in the `tab` slot are recognized as buttons without needing `ui-tab-button`. The component adds `role="tab"`, `aria-selected`, `aria-posinset`, and `tabindex` to each slotted tab. The internal tab container has `role="tablist"`.

When a panel is activated, the component adds an empty `open` attribute to it; it removes `open` from inactive panels. It hides inactive panels using inline `display: none` and restores each panel's original inline `display` value when activated.

## Properties

| Property | Behavior |
| --- | --- |
| `tabNumber` | Gets or sets the selected tab number, starting at 1. Setting it selects that tab. |
| `tabCount` | Gets the number of assigned tab elements. |

```js
const tabs = document.querySelector("ui-tab");

tabs.tabNumber = 2;
console.log(tabs.tabNumber); // 2
console.log(tabs.tabCount);
```

`tabNumber` is not range-checked. Set it to a number from 1 through `tabCount`; an out-of-range value can leave every tab unselected and every panel hidden. Setting the current number again is a no-op and does not dispatch an event.

## Keyboard and events

- Clicking a slotted tab selects it.
- `ArrowLeft` and `ArrowUp` select the previous tab.
- `ArrowRight` and `ArrowDown` select the next tab.
- Arrow navigation stops at either end; it does not wrap.
- When arrow navigation changes tabs, the newly selected tab receives focus. `Home` and `End` are not handled.

Whenever the selected index changes, the component dispatches a bubbling, composed `tabselected` event from the `<ui-tab>` element. This also happens for programmatic changes and the component's initial selection, not only for user interaction. The event has no custom `detail`; read `tabNumber` from the component.

```js
tabs.addEventListener("tabselected", (event) => {
  console.log(event.target.tabNumber);
});
```

The component listens for changes to the panel slot, but not the tab slot. Add or remove tabs and panels before initialization where possible; changing slotted children after a selection has been made does not automatically reconcile the selected index or the tab/panel counts.

## CSS customization

Set these custom properties on `<ui-tab>` or an ancestor. Values below are the defaults documented by the component stylesheet; the panel padding is also used directly by the implementation.

| Variable | Default |
| --- | --- |
| `--ui-tab-label-background` | `var(--ui-background, hsl(0 0% 97% / 1))` |
| `--ui-tab-label-foreground` | `var(--ui-foreground, hsl(0 0% 18% / 1))` |
| `--ui-tab-label-hover-background` | `var(--ui-midground, hsl(0 0% 90% / 1))` |
| `--ui-tab-label-hover-foreground` | `var(--ui-foreground, hsl(0 0% 18% / 1))` |
| `--ui-tab-label-padding` | `8px 16px` |
| `--ui-tab-label-radius` | `var(--ui-border-radius-large, 6px)` |
| `--ui-tab-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-tab-font-weight` | `var(--ui-font-weight-normal, normal)` |
| `--ui-tab-boxed-label-background` | `var(--ui-midground, hsl(0 0% 90% / 1))` |
| `--ui-tab-boxed-label-foreground` | `var(--ui-foreground, hsl(0 0% 18% / 1))` |
| `--ui-tab-boxed-label-selected-background` | `var(--ui-background, hsl(0 0% 97% / 1))` |
| `--ui-tab-boxed-label-selected-foreground` | `var(--ui-foreground, hsl(0 0% 18% / 1))` |
| `--ui-tab-boxed-label-padding` | `8px 16px` |
| `--ui-tab-boxed-label-radius` | `var(--ui-border-radius-large, 6px)` |
| `--ui-tab-button-padding` | `8px` |
| `--ui-tab-boxed-border-radius` | `var(--ui-border-radius-large, 6px)` |
| `--ui-tab-boxed-border-style` | `solid` |
| `--ui-tab-boxed-border-width` | `2px` |
| `--ui-tab-boxed-border-color` | `var(--ui-midground, hsl(0 0% 90% / 1))` |
| `--ui-tab-boxed-panel-background` | `var(--ui-background, hsl(0 0% 97% / 1))` |
| `--ui-tab-boxed-panel-foreground` | `var(--ui-foreground, hsl(0 0% 18% / 1))` |
| `--ui-tab-bar-style` | `solid` |
| `--ui-tab-bar-width` | `2px` |
| `--ui-tab-bar-color` | `var(--ui-midground, hsl(0 0% 90% / 1))` |
| `--ui-tab-bar-selected-color` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-tab-bar-gap` | `4px` |
| `--ui-tab-panel-background` | `var(--ui-background, hsl(0 0% 97% / 1))` |
| `--ui-tab-panel-foreground` | `var(--ui-foreground, hsl(0 0% 18% / 1))` |
| `--ui-tab-panel-padding` | `0` |
| `--ui-tab-gap` | `8px` |

The unboxed panel colors apply without `boxed`; the boxed panel colors apply with it. Label hover colors apply to built-in text labels on devices that support hover. The bar variables style the selected indicator and, with `bar`, the separators between tabs.
