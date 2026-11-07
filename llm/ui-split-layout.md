# `<ui-split-layout>`

`<ui-split-layout>` arranges its direct child elements as resizable panels in a CSS grid. Panels are laid out horizontally by default; add `vertical` to stack them vertically. Drag the divider between adjacent panels to resize them.

## Import

```js
import ".../ui-web/ui-split-layout.js";
```

## Usage

Each direct child element is a panel. Give the component a definite width and height so its grid and panel sizes can be measured. Child elements should be able to shrink; the component's stylesheet sets slotted children to `min-width: 0` and `min-height: 0`.

```html
<ui-split-layout style="width: 100%; height: 24rem;" bar dots>
  <section>Navigation</section>
  <section>Content</section>
  <section>Details</section>
</ui-split-layout>
```

The `size` attribute sets initial CSS grid track sizes in panel order:

```html
<ui-split-layout style="height: 20rem;" size="1fr 2fr">
  <section>One third</section>
  <section>Two thirds</section>
</ui-split-layout>
```

The size list is space-separated and should have one value per panel. If omitted, panels start with equal `1fr` tracks. The attribute is an initial layout setting; it is not rewritten when users drag dividers or when `setSizeList()` is called.

### Vertical layout

```html
<ui-split-layout vertical style="height: 30rem;" bar>
  <section>Top</section>
  <section>Bottom</section>
</ui-split-layout>
```

For horizontal layouts, set the host width; for vertical layouts, set its height. The component uses the corresponding panel widths or heights while resizing.

### Per-panel divider settings

The divider after each panel is controlled by settings on that panel. The last panel has no divider after it.

```html
<ui-split-layout>
  <section split-layout-bar split-layout-dots split-layout-min="120">Navigation</section>
  <section>Content</section>
  <section>Details</section>
</ui-split-layout>
```

The `split-layout-bar`, `split-layout-dots`, and `split-layout-min` attributes on a panel enable a visible divider, show dots, or set that panel's minimum size. These child attributes can enable a setting from the host defaults; they do not turn off a setting enabled on the host.

Nested `<ui-split-layout>` elements can be used to divide a panel again. Configure the nested layout's direction and size independently.

## Attributes

| Attribute | Effect |
| --- | --- |
| `vertical` | Stacks panels in rows and resizes them by height. Without it, panels are in columns and resize by width. |
| `bar` | Shows a visible divider for each panel boundary. Without it, a narrow hidden divider remains draggable. |
| `dots` | Shows three dots in visible dividers. Requires `bar`. |
| `min` | Default minimum width (horizontal) or height (vertical) in pixels for each panel. Defaults to `16`; values below `16` are raised to `16`. |
| `size` | Space-separated CSS grid track values for the initial panel sizes, in child order. Defaults to equal `1fr` tracks. |

Each direct child panel can also use:

| Attribute | Effect |
| --- | --- |
| `split-layout-bar` | Enables a visible divider after this panel. |
| `split-layout-dots` | Enables dots in the visible divider after this panel. |
| `split-layout-min` | Overrides the default minimum size for this panel in pixels; values below `16` are raised to `16`. |

`vertical`, `bar`, `dots`, `min`, and `size` are read when the layout is built. Changing these host attributes after connection does not rebuild the layout. Child panel insertion/removal and child attributes are watched and cause the layout to rebuild.

## Properties and methods

| API | Behavior |
| --- | --- |
| `getSizeList()` | Returns the current panel sizes as an array of rounded pixel measurements, in child order. |
| `setSizeList(sizeList)` | Sets panel sizes using an array of pixel values, then updates the layout. Values are applied in child order. |

```js
const layout = document.querySelector("ui-split-layout");

console.log(layout.getSizeList());
layout.setSizeList([240, 480, 240]);
```

After dragging, the component stores the resulting panel sizes as pixel measurements, but uses proportional `fr` tracks for the final grid layout. The `size` attribute remains unchanged.

## Events

The `changed` event bubbles and is composed. It fires when a divider drag ends, including when `Escape` ends the drag. It does not fire for `setSizeList()`.

```js
layout.addEventListener("changed", (event) => {
  const beforePanel = event.detail.beforeElement;
  const afterPanel = event.detail.afterElement;
  console.log(beforePanel, afterPanel);
});
```

`event.detail` contains:

| Property | Value |
| --- | --- |
| `beforeElement` | Panel before the dragged divider in DOM order. |
| `afterElement` | Panel after the dragged divider in DOM order. |

Pressing `Escape` while dragging ends the drag; it does not restore the sizes from before the drag began.

## CSS customization

Set these variables on `<ui-split-layout>` or an ancestor:

| Variable | Default |
| --- | --- |
| `--ui-split-layout-bar-size` | `8px` |
| `--ui-split-layout-bar-hide-size` | `2px` |
| `--ui-split-layout-bar-interface-size` | `16px` |
| `--ui-split-layout-bar-color` | `var(--ui-background, hsl(0 0% 97% / 1))` |
| `--ui-split-layout-bar-radius` | `4px` |
| `--ui-split-layout-bar-hide-color` | `var(--ui-background, hsl(0 0% 97% / 1))` |
| `--ui-split-layout-bar-hover-color` | 50/50 HSL color mix of selected foreground and background |
| `--ui-split-layout-bar-hover-radius` | `4px` |
| `--ui-split-layout-bar-hover-delay` | `0.5s` |
| `--ui-split-layout-bar-selected-color` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-split-layout-bar-selected-radius` | `4px` |
| `--ui-split-layout-dot-size` | `4px` |
| `--ui-split-layout-dot-color` | 50/50 HSL color mix of foreground and background |
| `--ui-split-layout-dot-gap` | `3px` |

`bar-interface-size` sets the invisible pointer target size, so dividers can remain easy to drag even when the visible bar is narrow or hidden. Bar width is used horizontally; bar height is used vertically. Dot size should fit within the visible bar.
