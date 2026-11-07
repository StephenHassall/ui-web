# `<ui-pie-chart>`

`<ui-pie-chart>` renders category/value data as a pie, donut, or half-pie/half-donut. It supports labels, popups, selection/highlighting, and a linked `<ui-chart-legend>`.

```js
import ".../ui-web/ui-pie-chart.js";
```

Give it a measurable width and height, then call `setChart(dataList, options)`:

```html
<ui-pie-chart id="market-share" style="display: block; width: 100%; height: 360px;"></ui-pie-chart>
```

```js
const chart = document.querySelector("#market-share");

chart.setChart(
  [
    { category: "Alpha", value: 42, color: "#1858bb" },
    { category: "Beta", value: 28, color: "#e1b038" },
    { category: "Other", value: 30 },
  ],
  {
    highlight: true,
    selectable: true,
    label: { show: true, type: "percentage" },
    popup: { show: true },
  },
);
```

The chart uses the smaller of the available dimensions to size the pie/donut, leaving unused space on the longer axis when the host is not square. Half charts use a wide aspect ratio for top/bottom halves and a tall one for left/right halves.

## Data format

Pass an array of category objects with a positive numeric `value` and a `category`. Optional fields:

| Field | Description |
|---|---|
| `category` | Category label used by the default label, popup, and legend formatters. |
| `value` | Positive numeric amount; determines the slice angle and percentage. |
| `color` | CSS color for this slice. If omitted, a chart palette color is used. |
| `text` | Optional display text used by the default label and popup value formatters instead of the calculated value/percentage text. |
| `data` | Optional application data copied through with the item. |

```js
[
  { category: "A", value: 12, text: "12 units", data: { id: "a" } },
  { category: "B", value: 8, color: "var(--brand-blue)" },
]
```

Number entries are also accepted as shorthand: `[12, 8, 4]`. They use an empty category and the number as both the value and data field, so object entries are usually preferable.

The component does not aggregate duplicate categories. Slice order follows the input order, and default colors cycle through 20 colors. Shared `--ui-chart-color1` through `--ui-chart-color20` variables can customize the palette.

## Options

Only include options that differ from their defaults.

### Shape and interaction

| Option | Default | Description |
|---|---:|---|
| `type` | `"pie"` | `"pie"`, `"donut"`, `"pie-half"`, or `"donut-half"`. |
| `halfType` | `"top"` | Half-chart orientation: `"top"`, `"bottom"`, `"left"`, or `"right"`. Applies to half chart types. |
| `selectable` | `false` | Allow selecting slices with a click. |
| `highlight` | `false` | Highlight slices on pointer hover. |
| `padding` | `12` | Outer padding in pixels. |
| `donutSize` | `32` | Donut ring thickness in pixels. |
| `selectableSize` | `5` | Selection ring thickness in pixels. |
| `highlightSize` | `10` | Highlight ring thickness in pixels. |

### Legend

| Option | Default | Description |
|---|---|---|
| `legend.showPopup` | `false` | Show the chart popup when a linked legend item highlights a slice. |
| `legend.formatTextCallback` | `category => category` | Format linked legend labels; called with the category. |

### Labels

| Option | Default | Description |
|---|---:|---|
| `label.show` | `false` | Show text inside/on slices. |
| `label.showLimit` | `4` | Hide labels for slices below this percentage threshold. |
| `label.type` | `"percentage"` | `"percentage"`, `"value"`, or `"category"`. |
| `label.textPrefix`, `label.textSuffix` | `""` | Surround value labels; used for `type: "value"`. |
| `label.textGap` | `16` | Distance of pie labels from the slice's outer edge. |
| `label.formatTextCallback` | built-in formatter | Return a label for a data item; called with the item. |

The default percentage label rounds to a whole percent. A data item's truthy `text` overrides the built-in label text regardless of label type. The formatter can be used to customize labels:

```js
label: {
  show: true,
  type: "value",
  textPrefix: "$",
  formatTextCallback: (item) => `${item.category}: $${item.value.toFixed(2)}`,
}
```

### Popup

| Option | Default | Description |
|---|---|---|
| `popup.show` | `false` | Show a popup while hovering a slice. |
| `popup.formatInfoTextCallback` | `item => item.category` | Format popup info; called with the data item. |
| `popup.formatValueTextCallback` | value and percentage formatter | Format popup value; called with the data item. |

By default, the popup value is `"value (percentage%)"`. A truthy `item.text` replaces that text. Popup display and pointer highlighting require pointer interaction.

### Donut center slots

Donut and half-donut charts expose `small` and `large` slots. Place content in the chart's center:

```html
<ui-pie-chart style="width: 320px; height: 320px;">
  <div slot="small">42%</div>
</ui-pie-chart>
```

The `small` slot fits the hole without being covered by the ring. The `large` slot extends beneath the ring, whose inner edge masks its corners. These slots are hidden for pie charts.

## Methods and events

### `chartItemSelect(index)` and `chartItemHighlight(index)`

Use a zero-based data-item index to select or highlight a slice programmatically. Pass `null` to clear. Selection requires `selectable: true`; highlighting requires `highlight: true`.

### `getColorList()`

Returns the list of colors assigned to the valid data items after `setChart()`.

### `selected` and `highlighted` events

These bubbling, composed custom events are dispatched on user interaction (and when the corresponding method is called for a valid slice). `event.detail.index` is the zero-based data-item index:

```js
chart.addEventListener("selected", ({ detail }) => {
  console.log(detail.index);
});

chart.addEventListener("highlighted", ({ detail }) => {
  console.log(detail.index);
});

chart.chartItemHighlight(0);
chart.chartItemSelect(null);
```

## Link a chart legend

Give the chart an `id` and use the same value in the legend's `for` attribute. The chart builds one legend item per valid slice when `setChart()` runs:

```html
<ui-pie-chart id="market-share" style="height: 320px;"></ui-pie-chart>
<ui-chart-legend for="market-share"></ui-chart-legend>
```

The chart searches the surrounding DOM/composed-tree path for the matching legend, so an `id` is required and separate shadow roots can prevent the link from being found. Legend selection/highlighting only affects the chart when `selectable`/`highlight` is enabled. `legend.showPopup` controls whether a legend-triggered highlight also displays the popup.

## CSS custom properties

Set these properties on the chart or an ancestor:

| Area | Properties and defaults |
|---|---|
| Background | `--ui-pie-chart-background`: `var(--ui-background, hsl(0 0% 97% / 1))`; `--ui-pie-chart-inside`: same theme background for the donut center. |
| Slice gaps | `--ui-pie-chart-gap-linejoin`: `round`; `--ui-pie-chart-gap-width`: `1px`; `--ui-pie-chart-gap-color`: theme background. |
| Selection | `--ui-pie-chart-select-opacity`: `1`; `--ui-pie-chart-select-filter`: `none`. |
| Highlight | `--ui-pie-chart-highlight-opacity`: `0.8`; `--ui-pie-chart-highlight-filter`: `brightness(125%)`. |
| Labels | `--ui-pie-chart-label-font-size`: small / `12px`; `--ui-pie-chart-label-font-weight`: normal; `--ui-pie-chart-label-color`: `hsl(0 0% 97% / 1)`. |
| Animation | `--ui-pie-chart-animation-show-duraction`: `var(--ui-animation-duration-default, 200ms)`. The variable is spelled `duraction`. |
| Popup colors and typography | `--ui-pie-chart-popup-background`: foreground; `-foreground`: background; info font size small / `12px`; value font size normal / `16px`; info weight normal; value weight bold / `600`. |
| Popup layout | `--ui-pie-chart-popup-padding`: `4px`; `-row-gap`: `0`; `-column-gap`: `4px`; `-tab`: `8px`; `-radius`: small / `3px`; `-min-width`: `100px`; `-max-width`: `150px`; `-triangle-size`: `8px`. |

## Implementation notes

- Object data entries with `value: 0` are discarded because the implementation checks the value for truthiness. Negative values are also discarded. Use strictly positive values; zero-sized categories will not appear.
- The implementation does not validate that object values are finite numbers. Invalid values can corrupt the total and slice proportions; provide finite positive numbers.
- `setChart()` processes new data and options but schedules a redraw only before the chart has initialized its dimensions. Calling it again after initial rendering can leave the SVG stale until the chart resizes.
- Pie slices are SVG pointer targets, but the component does not provide equivalent keyboard selection or accessible names for the slices. Include a text/table equivalent when the values matter.
