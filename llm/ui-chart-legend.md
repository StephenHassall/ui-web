# `<ui-chart-legend>`

`<ui-chart-legend>` displays the legend items for a chart. Import it with:

```js
import ".../ui-web/ui-chart-legend.js";
```

## Link it to a chart

Give the chart an `id` and set the legend's `for` attribute to the same value. The chart fills the legend when its `setChart(data, options)` method runs:

```html
<div class="chart-layout">
  <ui-pie-chart id="revenue-chart"></ui-pie-chart>
  <ui-chart-legend for="revenue-chart"></ui-chart-legend>
</div>
```

```js
const chart = document.querySelector("#revenue-chart");
chart.setChart(
  [
    { category: "Product A", value: 42 },
    { category: "Product B", value: 28 },
  ],
  {
    highlight: true,
    selectable: true,
    legend: { showPopup: true },
  },
);
```

The chart builds the legend entries and supplies their colors and labels. For a bar chart, `legend.type` controls whether entries represent categories (`0`, the default), series (`1`), or individual category/series values (`2`). Pie charts create one entry per category.

The chart searches for a matching `<ui-chart-legend for="...">` in its surrounding DOM/composed-tree lookup path, not across the whole document. Keep the chart and its legend in a DOM area where the chart can find the legend; separate component shadow roots can prevent the link from being found. An `id` is required on the chart.

The chart's `selectable` and `highlight` options control whether legend clicks select chart items and pointer movement highlights them. `legend.showPopup` can enable a chart popup when highlighting from the legend.

## Orientation and layout

The default layout is vertical. Add `horizontal` to display items in a horizontal, wrapping row layout:

```html
<ui-chart-legend for="revenue-chart" horizontal></ui-chart-legend>
```

The internal list fills the legend element's height. Give the component adequate space; a constrained vertical height can wrap items into additional columns. The horizontal layout wraps into additional rows.

## Manual population

`setChart(dataList, chartElement)` is also available for directly populating a legend. Call it after the custom element has been defined. Each item needs `color` and `legend`; provide `index` and the chart element to enable interaction with a chart:

```js
await customElements.whenDefined("ui-chart-legend");

const legend = document.querySelector("ui-chart-legend");
const chart = document.querySelector("#revenue-chart");

legend.setChart(
  [
    { color: "#1858bb", legend: "Product A", index: 0 },
    { color: "#e1b038", legend: "Product B", index: 1 },
  ],
  chart,
);
```

Without a chart element, the items still render but clicks and pointer highlighting do nothing. The chart element must provide `chartItemSelect(index)` and `chartItemHighlight(index)` methods for the corresponding interactions to work.

## CSS custom properties

Set these properties on the legend or an ancestor:

| Property | Default | Description |
|---|---|---|
| `--ui-chart-legend-background` | `var(--ui-background, hsl(0 0% 90% / 1))` | Legend and item background. |
| `--ui-chart-legend-text-color` | `var(--ui-foreground, hsl(0 0% 18% / 1))` | Legend text color. |
| `--ui-chart-legend-text-font-size` | `var(--ui-font-size-normal, 16px)` | Text size. |
| `--ui-chart-legend-text-font-weight` | `var(--ui-font-weight-normal, normal)` | Text weight. |
| `--ui-chart-legend-color-size` | Help documents `16px` | Listed in help, but not applied by the current stylesheet; see implementation note below. |
| `--ui-chart-legend-color-border-width` | `2px` | Swatch border width. |
| `--ui-chart-legend-color-border-radius` | `3px` | Swatch corner radius. |
| `--ui-chart-legend-padding` | `4px` | Padding around the list. |
| `--ui-chart-legend-row-gap` | `8px` | Gap between rows/items. |
| `--ui-chart-legend-column-gap` | `16px` | Gap between columns/items. |
| `--ui-chart-legend-color-gap` | `8px` | Gap between a swatch and its label. |

## Implementation notes

- The component has no automatic reconnect handler. Its item event listeners are removed when it is detached. If a populated legend is detached and then reattached, call `setChart()` again to restore click/highlight interaction.
