# `<ui-day-chart>`

`<ui-day-chart>` renders a calendar-style heat map with one cell per day. It supports horizontal and vertical layouts, configurable color scales, optional month/week axes, popups, and integration with `<ui-chart-legend>`.

```js
import ".../ui-web/ui-day-chart.js";
```

Create the chart by calling `setChart(fromDate, toDate, dataList, options)` with JavaScript `Date` objects:

```html
<ui-day-chart id="activity-chart"></ui-day-chart>
```

```js
const chart = document.querySelector("#activity-chart");

chart.setChart(
  new Date(2026, 0, 1),
  new Date(2026, 3, 1),
  [
    { date: new Date(2026, 0, 3), value: 1 },
    { date: new Date(2026, 0, 4), value: 3, text: "Three events" },
  ],
  {
    cell: { fromValue: 0, toValue: 5 },
    popup: { show: true },
  },
);
```

The chart has a fixed pixel size calculated from the date range and options; it does not stretch to fill a responsive container. Use a horizontally scrollable wrapper for long ranges if needed. The default chart orientation is horizontal.

## Date range and data

- `fromDate` and `toDate` must both be valid `Date` objects, and `fromDate` must be earlier than `toDate`. The end date is included in the chart.
- Each data item must have a `date` that is a `Date` object and a numeric `value`. Optional `text` replaces the default popup value text.
- Data values outside `cell.fromValue` to `cell.toValue` are discarded. Data outside the displayed date range will not correspond to a cell.
- Use one item per calendar day. Duplicate dates are accepted by the data processing but the chart does not aggregate them; lookup can display only one of the duplicates.
- Use dates representing local calendar days (for example `new Date(year, monthIndex, day)`). The implementation lays out dates using local date fields but serializes cell dates with `toISOString()`, so UTC-offset boundaries can affect displayed date labels/highlighting.
- With `fillWeek: true`, the grid expands to full weeks around the selected range. The implementation adjusts the supplied `fromDate`/`toDate` objects in place; pass fresh `Date` objects if you need to preserve their original values.

## Options

Only include settings that differ from their defaults.

### General and axes

| Option | Default | Description |
|---|---:|---|
| `type` | `"horizontal"` | `"horizontal"` lays out weeks as columns and weekdays as rows. `"vertical"` rotates the overall arrangement. |
| `padding` | `12` | Outer padding in pixels. |
| `fillWeek` | `false` | Extend the date grid to complete the first and last weeks. |
| `monthAxis.show` | `true` | Show month labels. |
| `monthAxis.size` | `30` horizontal; `40` vertical | Month-axis width in vertical layout or height in horizontal layout. |
| `monthAxis.switch` | `false` | Move the month axis to the opposite side. |
| `monthAxis.textType` | `"short"` | Month label style; date formatting supports `"long"`, `"short"`, `"narrow"`, `"2-digit"`, and `"numeric"`. |
| `monthAxis.rotateText` | `false` | Rotate month labels. |
| `monthAxis.textGap` | `8` | Gap between month labels and cells. |
| `monthAxis.formatTextCallback` | built-in formatter | Format a month label; called with `(month, year)`. |
| `weekAxis.show` | `true` | Show weekday labels. |
| `weekAxis.size` | `40` horizontal; `30` vertical | Week-axis width in horizontal layout or height in vertical layout. |
| `weekAxis.switch` | `false` | Move the week axis to the opposite side. |
| `weekAxis.startOfWeek` | `"sun"` | First weekday; accepts names/abbreviations beginning `sun`, `mon`, `tue`, `wed`, `thu`, `fri`, or `sat`. |
| `weekAxis.textType` | `"short"` horizontal; `"narrow"` vertical | Weekday label style: `"long"`, `"short"`, or `"narrow"`. |
| `weekAxis.rotateText` | `false` | Rotate weekday labels. |
| `weekAxis.showSundayText` | `false` | Show Sunday label. |
| `weekAxis.showMondayText` | `true` | Show Monday label. |
| `weekAxis.showTuesdayText` | `false` | Show Tuesday label. |
| `weekAxis.showWednesdayText` | `true` | Show Wednesday label. |
| `weekAxis.showThursdayText` | `false` | Show Thursday label. |
| `weekAxis.showFridayText` | `true` | Show Friday label. |
| `weekAxis.showSaturdayText` | `false` | Show Saturday label. |
| `weekAxis.textGap` | `8` | Gap between weekday labels and cells. |
| `weekAxis.formatTextCallback` | built-in formatter | Format a weekday label; called with `dayOfWeek` (`0` Sunday through `6` Saturday). |

### Cells and colors

| Option | Default | Description |
|---|---:|---|
| `cell.fromValue` | `0` | Lowest accepted data value and start of the color scale. |
| `cell.toValue` | `1` | Highest accepted data value and end of the color scale. |
| `cell.emptyColor` | `"#b0bec5"` | Color for dates with no data. |
| `cell.fromColor` | `"#a6c4f5"` | Start color for a generated scale; must be `#RRGGBB`. |
| `cell.toColor` | `"#0047b7"` | End color for a generated scale; must be `#RRGGBB`. |
| `cell.colorCount` | `4` | Number of generated colors, including endpoints. |
| `cell.colorList` | `[]` | Explicit CSS color list; when nonempty, it takes precedence over generated colors. |
| `cell.size` | `12` | Cell width and height in pixels. |
| `cell.gap` | `4` | Gap between cells in pixels. |
| `cell.radius` | `2` | Cell corner radius in pixels. |

Data values are mapped proportionally to the generated/explicit color list. An empty date uses `emptyColor`, not the first scale color.

### Legend and popup

| Option | Default | Description |
|---|---|---|
| `legend.colorItemList` | `[]` | Optional entries shaped like `{ name }`, used for default legend labels by color index. |
| `legend.formatTextCallback` | built-in formatter | Return legend text for a color index; called with `(colorIndex)`. Empty/falsy labels are omitted. |
| `popup.show` | `false` | Show a popup while hovering a day cell. |
| `popup.formatInfoTextCallback` | date formatter | Format popup information; called with `(item, date)`. |
| `popup.formatValueTextCallback` | value/text formatter | Format popup value; called with `(item, date)`. |

For chart-legend integration, give the chart an `id` and add a matching legend where the chart can find it:

```html
<ui-day-chart id="activity" style="height: fit-content;"></ui-day-chart>
<ui-chart-legend for="activity"></ui-chart-legend>
```

The day chart supplies one legend item per color. To show legend entries, provide `legend.colorItemList` names and/or a `legend.formatTextCallback`; the default formatter returns no label for colors without a configured name. The linked legend can call the chart's no-op `chartItemSelect()` and `chartItemHighlight()` methods, so it does not select or highlight day-chart cells.

## Methods and event

### `chartDateHighlight(date)`

Show the popup over the cell for a `Date`; pass `null` to hide it. This only displays a popup when `popup.show` is enabled. The date must match a rendered cell.

```js
chart.chartDateHighlight(new Date(2026, 0, 3));
chart.chartDateHighlight(null);
```

### `getColorList()`

Return the color list used for cell values after `setChart()` has processed the options.

### `highlighted` event

Fired when pointer movement highlights a different cell. It bubbles and is composed across shadow DOM. `event.detail.date` is a `"YYYY-MM-DD"` string; `event.detail.value` is the data value, or `undefined` when that date has no data.

```js
chart.addEventListener("highlighted", ({ detail }) => {
  console.log(detail.date, detail.value);
});
```

## CSS custom properties

Set these on the chart or an ancestor:

| Property | Default | Description |
|---|---|---|
| `--ui-day-chart-animation-show-duraction` | `var(--ui-animation-duration-default, 200ms)` | SVG fade-in duration. The variable is spelled `duraction` in the implementation. |
| `--ui-day-chart-background` | `var(--ui-background, hsl(0 0% 97% / 1))` | SVG/chart background. |
| `--ui-day-chart-cell-width` | `1px` | Cell outline width. |
| `--ui-day-chart-week-axis-text-color` | `var(--ui-foreground, hsl(0 0% 18% / 1))` | Weekday label color. |
| `--ui-day-chart-week-axis-text-font-size` | `var(--ui-font-size-normal, 16px)` | Weekday label size. |
| `--ui-day-chart-week-axis-text-font-weight` | `var(--ui-font-weight-normal, normal)` | Weekday label weight. |
| `--ui-day-chart-month-axis-text-color` | `var(--ui-foreground, hsl(0 0% 18% / 1))` | Month label color. |
| `--ui-day-chart-month-axis-text-font-size` | `var(--ui-font-size-normal, 16px)` | Month label size. |
| `--ui-day-chart-month-axis-text-font-weight` | `var(--ui-font-weight-normal, normal)` | Month label weight. |
| `--ui-day-chart-popup-background` | `var(--ui-foreground, hsl(0 0% 18% / 1))` | Popup background. |
| `--ui-day-chart-popup-foreground` | `var(--ui-background, hsl(0 0% 90% / 1))` | Popup text color. |
| `--ui-day-chart-popup-info-font-size` | `var(--ui-font-size-small, 12px)` | Popup date/info text size. |
| `--ui-day-chart-popup-value-font-size` | `var(--ui-font-size-normal, 16px)` | Popup value text size. |
| `--ui-day-chart-popup-info-font-weight` | `var(--ui-font-weight-normal, normal)` | Popup date/info weight. |
| `--ui-day-chart-popup-value-font-weight` | `var(--ui-font-weight-bold, 600)` | Popup value weight. |
| `--ui-day-chart-popup-padding` | `4px` | Popup padding. |
| `--ui-day-chart-popup-row-gap` | `0` | Gap between popup text rows. |
| `--ui-day-chart-popup-column-gap` | `4px` | Gap between popup columns. |
| `--ui-day-chart-popup-tab` | `8px` | Popup color-marker column width. |
| `--ui-day-chart-popup-radius` | `var(--ui-border-radius-small, 3px)` | Popup corner radius. |
| `--ui-day-chart-popup-min-width` | `100px` | Popup minimum width when both date and value are shown. |
| `--ui-day-chart-popup-max-width` | `150px` | Popup maximum width. |
| `--ui-day-chart-popup-triangle-size` | `8px` | Popup pointer size. |

## Implementation notes

- `setChart()` silently returns without updating if either date is missing/not a `Date`, or if the start is not earlier than the end. Use valid increasing dates.
- For accessibility, provide an equivalent textual summary or data table; cells are SVG pointer targets without keyboard interaction or accessible day/value labels.
