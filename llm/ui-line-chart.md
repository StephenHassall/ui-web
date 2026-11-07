# `<ui-line-chart>`

`<ui-line-chart>` renders one or more numeric or date-based data series as lines, curves, or bars. It supports axes, selectable/highlightable series, crosshairs, popups, and a linked `<ui-chart-legend>`.

```js
import ".../ui-web/ui-line-chart.js";
```

Give the element a measurable width and height, then call `setChart(dataList, options)`:

```html
<ui-line-chart id="revenue-chart" style="display: block; width: 100%; height: 360px;"></ui-line-chart>
```

```js
const chart = document.querySelector("#revenue-chart");

chart.setChart(
  [
    {
      name: "Revenue",
      dataList: [
        { x: 2023, y: 120 },
        { x: 2024, y: 145 },
        { x: 2025, y: 138 },
      ],
    },
  ],
  {
    highlight: true,
    selectable: true,
    line: { showDots: true },
    popup: { show: true },
  },
);
```

## Data format

`dataList` is an array of line objects. Each line needs a `dataList` array of `{ x, y }` points. `x` must be a number or a `Date`, and all points across all lines must use the same x type. Each line's x values must be strictly increasing:

```js
[
  {
    name: "Series A",
    dataList: [
      { x: new Date(2025, 0, 1), y: 12 },
      { x: new Date(2025, 1, 1), y: 18 },
    ],
  },
]
```

Supply points in ascending x order; duplicate x values within a line are not accepted. The implementation skips an invalid line rather than sorting it. Use finite numeric `y` values. A line with no valid points is omitted.

Line metadata can be specified directly on each line (`name`, `color`, `mode`, and `fill`) or by position in `options.line.infoList`. If metadata is supplied in both places, `infoList` overrides the corresponding line properties.

`mode` supports `"line"` (default), `"curve"`, and `"bar"`. For `"curve"`, `smoothing` controls the curve shape; values around `0` to `0.25` are generally most useful. Set `fill: true` to fill the area beneath a line.

## Options

Only include options that differ from their defaults.

### General and legend

| Option | Default | Description |
|---|---:|---|
| `selectable` | `false` | Allow clicking lines/points to select them. |
| `highlight` | `false` | Highlight lines/points on pointer hover. |
| `padding` | `12` | Outer chart padding in pixels. |
| `legend.formatTextCallback` | `lineItem => lineItem.name` | Format linked legend labels; called with a line data object. |

### Y axis

| Option | Default | Description |
|---|---:|---|
| `yAxis.show` | `true` | Show the Y axis. |
| `yAxis.size` | `50` | Y-axis width in pixels. |
| `yAxis.endPaddingSize` | `8` | Extra space after the last segment. |
| `yAxis.minSegmentSize` | `32` | Minimum pixel spacing used to calculate segment intervals. |
| `yAxis.segmentDenominator` | unset | Explicit interval between segments. |
| `yAxis.nubSize` | `8` | Tick-mark length. |
| `yAxis.fixedMin`, `yAxis.fixedMax` | unset | Extend the computed range to these bounds when they enclose the data; useful for comparing charts. |
| `yAxis.showLine` | `true` | Show the axis line. |
| `yAxis.showNub` | `true` | Show tick marks. |
| `yAxis.showText` | `true` | Show segment labels. |
| `yAxis.showZero` | `true` | Show the zero label. |
| `yAxis.textPrefix`, `yAxis.textSuffix` | `""` | Surround formatted segment values. |
| `yAxis.textGap` | `8` | Gap between a label and its tick. |
| `yAxis.formatTextCallback` | built-in formatter | Format a value-axis segment; called with `(segmentValue, fixedPoint)`. |

The Y-axis range includes zero by default, even if all data values are positive or all are negative.

### X axis

| Option | Default | Description |
|---|---:|---|
| `xAxis.show` | `true` | Show the X axis. |
| `xAxis.size` | `50` | X-axis height in pixels. |
| `xAxis.endPaddingSize` | `8` | Extra space after the last segment. |
| `xAxis.minSegmentSize` | `60` | Minimum pixel spacing used to calculate segment intervals. |
| `xAxis.segmentDenominator` | unset | Explicit interval. For numeric x values, use a number; for dates, use a value such as `"1year"`, `"2month"`, `"3day"`, `"4hour"`, `"5minute"`, or `"6second"`. |
| `xAxis.nubSize` | `8` | Tick-mark length; `-1` fills the available segment space. |
| `xAxis.showLine` | `true` | Show the axis line. |
| `xAxis.showNub` | `true` | Show tick marks. |
| `xAxis.showText` | `true` | Show segment labels. |
| `xAxis.showZero` | `true` | Show the zero label for a numeric axis. |
| `xAxis.rotateText` | `false` | Rotate labels 90 degrees. |
| `xAxis.textPrefix`, `xAxis.textSuffix` | `""` | Surround numeric segment labels. |
| `xAxis.textGap` | `8` | Gap between a label and its tick. |
| `xAxis.formatNumberTextCallback` | built-in formatter | Format a numeric segment; called with `(segmentValue, fixedPoint)`. |
| `xAxis.formatDateTextCallback` | built-in formatter | Format a date segment; called with `(segmentValue, dateSegmentType)`. The date segment type is `1` year, `2` month, `3` day, `4` hour, `5` minute, or `6` second. |

### Lines, guide lines, popups, and crosshair

| Option | Default | Description |
|---|---:|---|
| `guideLine.showXAxis` | `true` | Show horizontal guide lines at Y-axis segments. |
| `guideLine.showYAxis` | `true` | Show vertical guide lines at X-axis segments. |
| `line.showDots` | `false` | Draw visible dots at data points. |
| `line.dotSize` | `8` | Visible dot size. |
| `line.interfaceDotSize` | `12` | Invisible pointer target size around points. |
| `line.infoList` | `[]` | Per-series metadata: `{ name, color, mode, smoothing, fill }`, matched to data lines by array index. |
| `popup.show` | `false` | Show popup on pointer interaction with a point/line. |
| `popup.formatInfoTextCallback` | line name | Format popup info; called with `(lineItem, dataItem)`. |
| `popup.formatValueTextCallback` | built-in formatter | Format popup value; called with `(lineItem, dataItem)`. |
| `crosshair.show` | `false` | Intended to enable the crosshair; see implementation note below. |
| `crosshair.showXAxis` | `true` | Show the vertical crosshair line. |
| `crosshair.showYAxis` | `true` | Show the horizontal crosshair line. |
| `crosshair.mode` | `"highlight"` | Show on `"highlight"` or `"select"`. |
| `crosshair.showDot` | `true` | Show the dot at the crosshair intersection. |
| `crosshair.dotSize` | `6` | Crosshair dot radius. |

`line.infoList` example:

```js
line: {
  showDots: true,
  infoList: [
    { name: "Actual", color: "#1858bb", mode: "line", fill: false },
    { name: "Forecast", color: "#e1b038", mode: "curve", smoothing: 0.15, fill: true },
  ],
}
```

When no color is specified, series use the shared `--ui-chart-color1` through `--ui-chart-color20` palette.

## Selection, highlighting, and events

`chartItemSelect(index)` and `chartItemHighlight(index)` use zero-based line indices; pass `null` to clear. They only work when the corresponding `selectable` or `highlight` option is enabled. Pointer interactions on points can include their point index; interactions on the line itself use data index `-1`.

The `selected` and `highlighted` custom events bubble and are composed across shadow DOM. Both provide:

```js
{
  lineIndex, // zero-based series index
  dataIndex, // zero-based point index, or -1 for the line segment
}
```

```js
chart.addEventListener("selected", ({ detail }) => {
  console.log(detail.lineIndex, detail.dataIndex);
});

chart.chartItemHighlight(0);
chart.chartItemSelect(null);
```

## Link a chart legend

Give the chart an `id` and use the same value in the legend's `for` attribute. The chart builds one legend item per valid data line when `setChart()` runs:

```html
<ui-line-chart id="population-chart" style="height: 320px;"></ui-line-chart>
<ui-chart-legend for="population-chart"></ui-chart-legend>
```

The chart searches the surrounding DOM/composed-tree path for the matching legend; an `id` is required. The legend uses `legend.formatTextCallback` for labels and calls the chart's selection/highlight methods when interacted with.

## CSS custom properties

Set properties on the chart or an ancestor.

| Area | Properties and defaults |
|---|---|
| Background and animation | `--ui-line-chart-background`: `--ui-background`, then `hsl(0 0% 97% / 1)`; `--ui-line-chart-animation-show-duraction`: `--ui-animation-duration-default`, then `200ms` (spelling is `duraction`). |
| Y axis | `--ui-line-chart-y-axis-line-color`, `-nub-color`, `-text-color`, `-zero-color`: foreground defaults; line/nub/zero widths `2px`; text font size `--ui-font-size-normal` / `16px`, weight normal. |
| X axis | `--ui-line-chart-x-axis-line-color`, `-nub-color`, `-text-color`, `-zero-color`: foreground defaults; line/nub/zero widths `2px`; text font size `--ui-font-size-normal` / `16px`, weight normal. |
| Guide lines | `--ui-line-chart-guide-line-y-axis-color` and `-x-axis-color`: midground defaults; widths `2px`; dash arrays `none`. |
| Crosshair | `--ui-line-chart-crosshair-line-color`: foreground; width `2px`; dash array comment says `4px 4px` but CSS fallback is `2px 2px`. `--ui-line-chart-crosshair-dot-color`: CSS fallback `hsl(0 50% 30% / 1)`; background `hsl(0 0% 100% / 1)`; width `3px` (the comments/help list different defaults). |
| Lines and dots | `--ui-line-chart-line-opacity`: `1`; `--ui-line-chart-line-size`: `2px`; `--ui-line-chart-interface-size`: `6px`; `--ui-line-chart-fill-opacity`: `0.5`; `--ui-line-chart-dot-opacity`: `1`. |
| Selection | `--ui-line-chart-select-opacity`: `0.8`; `--ui-line-chart-select-size`: `4px`; `--ui-line-chart-select-dasharray`: `none` (currently ineffective due to a CSS variable typo); `--ui-line-chart-select-filter`: foreground drop shadow. Dot selection uses `--ui-line-chart-dot-select-opacity` (`0.8`) and `--ui-line-chart-dot-select-filter` (foreground drop shadow). |
| Highlight | `--ui-line-chart-highlight-opacity`: `1`; `--ui-line-chart-highlight-size`: `4px`; `--ui-line-chart-highlight-dasharray`: `8px 8px`; `--ui-line-chart-highlight-filter`: `none`. Dot highlighting uses `--ui-line-chart-dot-highlight-opacity` (`1`) and `--ui-line-chart-dot-highlight-filter` (`none`). |
| Popup colors and typography | `--ui-line-chart-popup-background`: foreground; `-foreground`: background; info font size small / `12px`; value font size normal / `16px`; info weight normal; value weight bold / `600`. |
| Popup layout | `--ui-line-chart-popup-padding`: `4px`; `-row-gap`: `0`; `-column-gap`: `4px`; `-tab`: `8px`; `-radius`: small / `3px`; `-min-width`: `100px`; `-max-width`: `150px`; `-triangle-size`: `8px`. |

## Implementation notes

- Calling `setChart()` after the chart has already initialized its dimensions processes new data and options but does not schedule a redraw. A subsequent resize redraws it; to update data reliably, resize the chart or recreate it.
- The component validates that x values are consistently numeric or `Date` and strictly ascending, but does not verify that y values are numbers. Supply numeric y values to avoid malformed scales.
- Use a textual data table or summary alongside the chart when the exact values are important; chart interaction is pointer-oriented and does not provide equivalent keyboard navigation.
