# `<ui-bar-chart>`

`<ui-bar-chart>` draws vertical or horizontal bar charts, optionally mixing bars with line/curve series. It renders to SVG and resizes with its container. Import the component:

```js
import ".../ui-web/ui-bar-chart.js";
```

Give it a non-zero width and height, then call `setChart(dataList, options)`:

```html
<ui-bar-chart id="sales-chart" style="display: block; width: 100%; height: 360px;"></ui-bar-chart>
```

```js
const chart = document.querySelector("#sales-chart");

chart.setChart(
  [
    { category: "Q1", valueList: [12, 8] },
    { category: "Q2", valueList: [18, 11] },
    { category: "Q3", valueList: [15, 14] },
  ],
  {
    series: {
      valueList: [
        { name: "Online", color: "#1858bb", mode: "bar" },
        { name: "In store", color: "#e1b038", mode: "bar" },
      ],
    },
    selectable: true,
    highlight: true,
    label: { show: true },
    popup: { show: true },
  },
);
```

The chart needs sufficient room for its axes and padding. Its calculated minimum width/height is based on the configured axis sizes and chart padding; see the implementation notes about the minimum-height check.

## Data format

Pass an array of category objects. Each item needs a truthy `category` and either a numeric `value` or an array `valueList`:

```js
[
  { category: "Apples", value: 12 },
  { category: "Pears", value: 8 },
]
```

Use `valueList` for multiple values/series per category. `text` and `textList` optionally provide display text for values instead of formatting the numbers. An optional `data` field is copied through for use in callbacks.

```js
[
  { category: "Apples", valueList: [12, 8], textList: ["12 kg", "8 kg"] },
  { category: "Pears", valueList: [10, 6], textList: ["10 kg", "6 kg"] },
]
```

For multi-value charts, `series.valueList` describes each value index. Each series can be a `"bar"` (default), `"line"`, or `"curve"`. A curve uses its `smoothing` value. Bars can be stacked with `stack: true`; this applies only to bar series. Negative values are supported.

## Chart options

Options are optional; omitted settings use the defaults listed below.

### General and legend

| Option | Default | Description |
|---|---:|---|
| `type` | `"vertical"` | `"vertical"` or `"horizontal"`. |
| `selectable` | `false` | Allow selecting bars/series by clicking. |
| `highlight` | `false` | Highlight bars/series on pointer hover. |
| `stack` | `false` | Stack bar values within each category (does not stack line series). |
| `padding` | `12` | Outer chart padding in pixels. |
| `cornerSize` | `4` | Bar corner radius. |
| `plusColor` | unset | CSS color for positive bars, overriding normal series/category color. |
| `minusColor` | unset | CSS color for negative bars, overriding normal series/category color. |
| `legend.type` | `0` | Selection/highlight grouping: `0` category, `1` series, `2` individual category-series value. |
| `legend.showPopup` | `false` | Show a popup when highlighting from a linked `<ui-chart-legend>`. |
| `legend.formatTextCallback` | built-in formatter | Format legend labels; called with `(category, seriesName)` as applicable. |

When no series colors are given, the chart cycles through 20 default colors. These can be overridden with the shared `--ui-chart-color1` through `--ui-chart-color20` properties.

### Series

`series.valueList` is an array, with one entry per value position in each category:

| Series field | Default | Description |
|---|---|---|
| `name` | — | Series name. |
| `color` | generated | CSS color. |
| `mode` | `"bar"` | `"bar"`, `"line"`, or `"curve"`. |
| `smoothing` | `0.15` | Curve smoothing, normally between `0` and `1`; large values can look odd. |

```js
series: {
  valueList: [
    { name: "Revenue", mode: "bar" },
    { name: "Target", mode: "curve", smoothing: 0.15 },
  ],
}
```

### Value axis

| Option | Default | Description |
|---|---:|---|
| `valueAxis.show` | `true` | Show the value axis. |
| `valueAxis.size` | `50` | Axis width for vertical charts; height for horizontal charts. |
| `valueAxis.endPaddingSize` | `8` | Extra space after the last axis segment. |
| `valueAxis.minSegmentSize` | `32` vertical, `60` horizontal | Minimum pixel spacing when calculating axis segments. |
| `valueAxis.segmentDenominator` | unset | Manually choose the interval between axis segments. |
| `valueAxis.baseLineAdjustment` | `5` | Percentage adjustment to leave space between bars and category axis. |
| `valueAxis.nubSize` | `8` | Length of the tick marks. |
| `valueAxis.fixedMin`, `fixedMax` | unset | Extend the value axis to these bounds when data lies within them. |
| `valueAxis.showLine` | `true` | Show the axis edge line. |
| `valueAxis.showNub` | `true` | Show tick marks. |
| `valueAxis.showText` | `true` | Show segment values. |
| `valueAxis.rotateText` | `false` | Rotate axis values 90 degrees. |
| `valueAxis.showZero` | `true` | Show the zero value label. |
| `valueAxis.textPrefix`, `textSuffix` | `""` | Add text around each segment value. |
| `valueAxis.textGap` | `8` | Space between the value text and tick. |
| `valueAxis.formatTextCallback` | built-in formatter | Format a segment value; called with `(segmentValue, fixedPoint)`. |

### Category axis, bars, and guide lines

| Option | Default | Description |
|---|---:|---|
| `categoryAxis.show` | `true` | Show the category axis. |
| `categoryAxis.switch` | `false` | Switch category axis to top/right instead of bottom/left. |
| `categoryAxis.size` | `50` | Axis height for vertical charts; width for horizontal charts. |
| `categoryAxis.nubSize` | `8` | Category tick length. |
| `categoryAxis.nubCenter` | `false` | Center ticks within categories rather than between them. |
| `categoryAxis.textType` | `0` | Internal category text type setting. |
| `categoryAxis.textGap` | `8` | Gap around category labels. |
| `categoryAxis.showLine` | `true` | Show axis edge line. |
| `categoryAxis.showNub` | `true` | Show category ticks. |
| `categoryAxis.showText` | `true` | Show category labels. |
| `categoryAxis.rotateText` | `false` | Rotate category labels 90 degrees. |
| `bar.categoryGap` | `8` | Gap between categories. |
| `bar.valueGap` | `0` | Gap between bars within a category. |
| `guideLine.showValueAxis` | `true` | Show value-axis guide lines. |
| `guideLine.showCategoryAxis` | `true` | Show category-axis guide lines. |

### Labels, popups, and lines

| Option | Default | Description |
|---|---:|---|
| `label.show` | `false` | Show value labels on bars. |
| `label.textPrefix`, `textSuffix` | `""` | Add text around the displayed label. |
| `label.textGap` | `12` | Gap between label and bar edge. |
| `label.location` | `"end"` | `"start"`, `"end"`, `"middle"`, `"outside"`, `"plus"`, or `"minus"`. |
| `label.locationLimit` | `16` | Threshold used to move labels outside when a bar is too small. |
| `label.formatTextCallback` | built-in formatter | Format a label; called with `(value, dataItem, seriesIndex)`. |
| `popup.show` | `false` | Show a popup on hover over a bar or line point. |
| `popup.formatInfoTextCallback` | built-in formatter | Format popup info; called with `(dataItem, seriesValue)`. |
| `popup.formatValueTextCallback` | built-in formatter | Format popup value; called with `(value, seriesIndex, dataItem)`. |
| `line.showDots` | `true` | Show data-point dots on lines. |
| `line.dotSize` | `0` | Visible dot size. |
| `line.interfaceDotSize` | `12` | Invisible pointer target size around line points. |

## Selection, highlighting, and events

`chartItemSelect(index)` and `chartItemHighlight(index)` set the selected or highlighted item programmatically. `index` is interpreted according to `legend.type`: category index, series index, or the flattened category-series value index. Indices are zero-based. Pass `null` to clear. These methods do nothing unless `selectable` or `highlight` respectively is enabled.

User interaction dispatches bubbling, composed custom events:

| Event | When | `event.detail` |
|---|---|---|
| `selected` | A selectable bar or line is clicked. | `{ categoryIndex, seriesIndex }` |
| `highlighted` | A highlight-enabled bar or line is hovered. | `{ categoryIndex, seriesIndex }` |

The indices in event details identify the data category and value/series position. For a line series, the category index can be `-1` because the line itself is selected/highlighted rather than one point.

```js
chart.addEventListener("selected", ({ detail }) => {
  console.log(detail.categoryIndex, detail.seriesIndex);
});

chart.chartItemSelect(1);
chart.chartItemHighlight(null); // clear highlight
```

## Linking a chart legend

To connect a `<ui-chart-legend>`, give the chart an `id` and set the legend's `for` attribute to the same value. The chart builds legend entries according to `legend.type` when `setChart()` is called.

```html
<ui-bar-chart id="sales" style="height: 320px;"></ui-bar-chart>
<ui-chart-legend for="sales"></ui-chart-legend>
```

The legend component must be available as a custom element. The chart itself does not move data when a legend item is selected; it provides highlight/select integration.

## CSS custom properties

Properties may be set on the chart or an ancestor.

| Area | Properties and defaults |
|---|---|
| Background | `--ui-bar-chart-background`: `--ui-background`, then `hsl(0 0% 97% / 1)`. |
| Animation | `--ui-bar-chart-animation-show-duraction`: `--ui-animation-duration-default`, then `200ms` (note the spelling `duraction`). |
| Value axis | `--ui-bar-chart-value-axis-line-color`, `-nub-color`, `-text-color`: foreground defaults; line/nub widths `2px`; text size `--ui-font-size-normal` / `16px`; weight `--ui-font-weight-normal` / `normal`. |
| Category axis | `--ui-bar-chart-category-axis-line-color`, `-nub-color`, `-text-color`: foreground defaults; line/nub widths `2px`; text size `--ui-font-size-normal` / `16px`; weight `--ui-font-weight-normal` / `normal`. |
| Guide lines | `--ui-bar-chart-guide-line-value-axis-color` and `-category-axis-color`: midground defaults; widths `2px`; dash arrays `none`. `--ui-bar-chart-guide-line-zero-color`: foreground; width `2px`. |
| Bars | `--ui-bar-chart-bar-opacity`: `1`; `--ui-bar-chart-bar-stroke-color`: `transparent`; `--ui-bar-chart-bar-stroke-dasharray`: `none`; `--ui-bar-chart-bar-stroke-size`: `0`; `--ui-bar-chart-bar-filter`: `none`. |
| Bar selection | `--ui-bar-chart-select-opacity`: `1`; `--ui-bar-chart-select-color`: foreground; `--ui-bar-chart-select-dasharray`: `none`; `--ui-bar-chart-select-size`: `4px`; `--ui-bar-chart-select-filter`: foreground drop shadow. |
| Bar highlight | `--ui-bar-chart-highlight-opacity`: `1`; `--ui-bar-chart-highlight-color`: foreground; `--ui-bar-chart-highlight-dasharray`: `4px 4px`; `--ui-bar-chart-highlight-size`: `4px`; `--ui-bar-chart-highlight-filter`: foreground drop shadow. |
| Labels | `--ui-bar-chart-label-font-size`: small / `12px`; `--ui-bar-chart-label-font-weight`: normal; `--ui-bar-chart-label-inside-color`: `hsl(0 0% 97% / 1)`; `--ui-bar-chart-label-outside-color`: foreground. |
| Lines | `--ui-bar-chart-line-opacity`: `1`; `--ui-bar-chart-line-size`: `2px`; `--ui-bar-chart-line-interface-size`: `6px`. |
| Line selection | `--ui-bar-chart-line-select-opacity`: `0.8`; `--ui-bar-chart-line-select-size`: `4px`; `--ui-bar-chart-line-select-dasharray`: `none`; `--ui-bar-chart-line-select-filter`: foreground drop shadow. |
| Line highlight | `--ui-bar-chart-line-highlight-opacity`: `1`; `--ui-bar-chart-line-highlight-size`: `4px`; `--ui-bar-chart-line-highlight-dasharray`: `8px 8px`; `--ui-bar-chart-line-highlight-filter`: `none`. |
| Dots | `--ui-bar-chart-dot-opacity`: `1`; `--ui-bar-chart-dot-select-opacity`: `0.8`; `--ui-bar-chart-dot-select-filter`: foreground drop shadow; `--ui-bar-chart-dot-highlight-opacity`: `1`; `--ui-bar-chart-dot-highlight-filter`: `none`. |
| Popup colors/type | `--ui-bar-chart-popup-background`: foreground; `--ui-bar-chart-popup-foreground`: background; info font size small / `12px`, value font size normal / `16px`; info weight normal, value weight bold / `600`. |
| Popup layout | `--ui-bar-chart-popup-padding`: `4px`; `-row-gap`: `0`; `-column-gap`: `4px`; `-tab`: `8px`; `-radius`: small / `3px`; `-min-width`: `100px`; `-max-width`: `150px`; `-triangle-size`: `8px`. |

## Implementation notes

- `setChart()` processes new data/options, but its source only schedules a redraw in the initial zero-size path. Once the chart has been sized, calling `setChart()` again may leave the rendered chart stale until a resize occurs.
- `setChart()` expects the chart to have measurable dimensions; set its height explicitly and ensure its parent layout gives it width.
