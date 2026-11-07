# `<ui-table>`

`<ui-table>` lays out direct child elements as cells in a CSS grid. It supports optional header cells, row and column styling, resizable columns, row and header reordering, and programmatic table operations. It is not a native HTML `<table>` and does not create native table semantics such as `<tr>` or `<td>`.

## Import

```js
import ".../ui-web/ui-table.js";
```

## Basic usage

Cells are provided as direct child elements in row-major order (left to right, then top to bottom). Mark header cells with `slot="header"`. The component infers the column count from the number of header cells; without headers, set `columns`. The default column count is 1.

```html
<ui-table row-lines striped>
  <div slot="header">Name</div>
  <div slot="header">Role</div>

  <div>Ada</div>
  <div>Engineer</div>
  <div>Grace</div>
  <div>Scientist</div>
</ui-table>
```

Without headers, specify the number of columns:

```html
<ui-table columns="2">
  <div>First row, first cell</div>
  <div>First row, second cell</div>
  <div>Second row, first cell</div>
  <div>Second row, second cell</div>
</ui-table>
```

All data rows must have the same number of cells as the table has columns. Elements with any `slot` attribute are not treated as data cells; `slot="header"` elements are used as headers. The component groups complete sets of cells into rows. Avoid incomplete trailing rows; ungrouped cells are not displayed as a row.

## Attributes

| Attribute | Effect |
| --- | --- |
| `columns` | Column count when headers do not determine it. Parsed as an integer; values below 1 are normalized to 1. Header count takes precedence if it is greater. This is the only observed attribute and changing it rebuilds the table structure. |
| `vertical` | Gives the first cell of each data row subheader styling. |
| `boxed` | Draws a border around the table and between cells. |
| `rounded` | Rounds the outer cell corners. |
| `row-lines` | Adds horizontal separators between rows. |
| `column-lines` | Adds vertical separators between columns. |
| `striped` | Alternates the background of data rows. |
| `striped-column` | Applies alternate-column styling (the implementation styles odd-numbered columns). |
| `hover` | Applies hover styling to rows on devices that support hover; also gives the row a pointer cursor unless row drag is enabled. |
| `disabled` | Prevents pointer interaction with the table and applies a visual filter. It does not disable programmatic methods. |
| `sticky-header` | Makes the header row sticky at the top of its scroll container. |
| `sticky-column` | Makes the first cell of each row sticky at the inline start (left in left-to-right layouts). |
| `column-bars` | Enables pointer-drag handles between columns for resizing. |
| `column-widths` | Initial CSS grid column sizes, such as `2fr 1fr`, `12rem 1fr`, or `30% 70%`. After resizing, the component writes the updated sizes back to this attribute as percentages. |
| `drag-drop-row` | Enables pointer and keyboard row reordering. Rows can also be moved between compatible `<ui-table>` elements. |
| `drag-row-cells` | Comma-separated 1-based data-column numbers that can initiate row dragging, e.g. `drag-row-cells="1,3"`. If omitted, any cell can initiate a drag. |
| `drag-drop-header` | Enables pointer and keyboard reordering of header columns within this table. |
| `drag-header-cells` | With `drag-drop-header`, comma-separated 1-based header-column numbers that may be dragged, e.g. `drag-header-cells="2,3"`. |
| `column-drag-from` | With `drag-drop-header`, sets the first 1-based column that may be dragged; preceding columns cannot be dragged. |
| `drag-name` | Name identifying this table as a source or destination for cross-table row dragging. |
| `drag-accept` | Comma-separated `drag-name` values accepted for cross-table row drops when source and destination names differ. Tables with the same `drag-name` can exchange rows without this attribute. |

`columns` is parsed with `parseInt`; provide a valid positive integer. An invalid nonnumeric value can result in an invalid column count. Attributes that control presentation are styled through CSS attribute selectors; set behavioral options before use to avoid changing drag configuration while a drag is active.

## Programmatic API

All row and column numbers are 1-based. The `afterRow` and `afterColumn` insertion positions use `-1` to mean the start; when omitted, `addRow` appends.

| Method | Behavior |
| --- | --- |
| `getColumnCount()` | Returns the current number of columns. |
| `addColumn(afterColumn, elementList, headerElement?)` | Adds a column and one cell to each existing row. `elementList` must contain one cell per data row. `headerElement` is optional and used if headers exist. `-1` inserts at the start. |
| `deleteColumn(column)` | Deletes the specified column and its cells. |
| `moveColumn(fromColumn, afterColumn)` | Moves a column after the specified column; `-1` moves it to the start. |
| `getColumnNumber(element)` | Returns the 1-based column containing the element or a descendant, or `-1` if it is not found. |
| `getRowCount()` | Returns the number of data rows, excluding the header. |
| `addRow(elementList, afterRow?)` | Adds a row containing exactly one element per column. Omit `afterRow` to append, or pass `-1` to insert first. |
| `deleteRow(row)` | Deletes the specified data row. |
| `clearRows()` | Removes all data rows but leaves the header. |
| `moveRow(fromRow, afterRow)` | Moves a data row after the given row; `-1` moves it to the start. |
| `selectRow(row)` | Applies selected-row styling in non-vertical mode. Multiple rows may be selected at once. |
| `unselectRow(row)` | Removes the selected state from the specified row. |
| `getRowElementList(row)` | Returns the row's assigned cell elements, or `null` if the row does not exist. |
| `getRowNumber(element)` | Returns the 1-based data row containing the element or a descendant, or `-1` if not found or in a header. |
| `setHeaders(elementList)` | Replaces the header cells and sets the column count to the number of provided elements. |
| `getHeaderElementList()` | Returns the header cells, or `null` when no header exists. |
| `getHeaderElement(column)` | Returns the header cell for a column, or `null` if not found. |
| `replaceHeaderElement(column, element)` | Replaces a header cell. |
| `getElement(row, column)` | Returns the cell at the given data row and column, or `null` if not found. |
| `replaceElement(row, column, element)` | Replaces the cell at the given data row and column. |
| `clear()` | Removes all light-DOM child content, including headers and data cells. |

`addRow` throws an error if its cell count does not equal the table's column count. `addColumn` likewise requires one new cell for each existing data row. Replacement and deletion methods expect valid 1-based indexes; check counts or getters before operating on dynamic tables.

```js
const table = document.querySelector("ui-table");

const firstRow = table.getRowElementList(1);
console.log(firstRow?.[0]?.textContent);

const newCells = [document.createElement("div"), document.createElement("div")];
newCells[0].textContent = "Lin";
newCells[1].textContent = "Designer";
table.addRow(newCells);
```

## Events

| Event | When it fires | `detail` |
| --- | --- | --- |
| `rowmoved` | A row is moved by pointer or keyboard drag-and-drop, within this table or to another compatible table. It is dispatched on the source table. Programmatic `moveRow()` does not fire it. | `fromTable`, `toTable`, `fromRow`, `toRow` |
| `columnmoved` | A header column is moved by pointer or keyboard drag-and-drop within the table. Programmatic `moveColumn()` does not fire it. | `table`, `fromColumn`, `toColumn` |
| `columnsized` | A column width is changed using a resize handle. | No custom detail; read the updated `column-widths` attribute. |

All three events bubble and are composed. Row and column positions in event details are 1-based.

```js
table.addEventListener("rowmoved", (event) => {
  const { fromTable, toTable, fromRow, toRow } = event.detail;
  console.log(fromTable, fromRow, "moved to", toTable, toRow);
});

table.addEventListener("columnsized", () => {
  console.log(table.getAttribute("column-widths"));
});
```

## Drag-and-drop interaction

- With `drag-drop-row`, focus a row and press **Space** to start keyboard dragging. Use arrow keys to reposition it; **Space** drops it and **Escape** exits the drag. Pointer dragging is also supported.
- With `drag-drop-header`, press **Space** on a draggable header, use **Left/Right Arrow** to move it, then press **Space** to finish; **Escape** also ends the keyboard operation. Pointer dragging is supported.
- **Escape** ends a row keyboard drag at its current drop position; it does not cancel or restore the original position.
- Cross-table row drops require both tables to have a `drag-name`. Matching names allow the drop; otherwise, the destination's `drag-accept` list must include the source table's name.
- During a row drag, the source/destination tables receive temporary `dragging-from`, `dragging-to`, and, for an allowed drop, `dragging-valid` attributes. These are removed when the drag finishes.

`disabled` blocks pointer interaction through CSS, but does not disable the keyboard drag handlers. Also, although `selectRow()` and `unselectRow()` still update row state in `vertical` mode, the built-in selected-row color styling is only applied in non-vertical mode.

## CSS customization

Set these custom properties on `<ui-table>` or an ancestor. They correspond to the defaults declared in the component stylesheet.

| Variable | Default |
| --- | --- |
| `--ui-table-background` | `var(--ui-background, hsl(0 0% 97% / 1))` |
| `--ui-table-foreground` | `var(--ui-foreground, hsl(0 0% 18% / 1))` |
| `--ui-table-header-background` | 70% table foreground mixed with 30% table background |
| `--ui-table-header-foreground` | Table background |
| `--ui-table-vertical-background` | 20% table foreground mixed with 80% table background |
| `--ui-table-vertical-foreground` | Table foreground |
| `--ui-table-row-line-style` | `solid` |
| `--ui-table-row-line-width` | `1px` |
| `--ui-table-row-line-color` | 30% table foreground mixed with 70% table background |
| `--ui-table-column-line-style` | `solid` |
| `--ui-table-column-line-width` | `1px` |
| `--ui-table-column-line-color` | 30% table foreground mixed with 70% table background |
| `--ui-table-header-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-table-header-font-weight` | `var(--ui-font-weight-bold, 600)` |
| `--ui-table-vertical-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-table-vertical-font-weight` | `var(--ui-font-weight-normal, normal)` |
| `--ui-table-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-table-font-weight` | `var(--ui-font-weight-normal, normal)` |
| `--ui-table-header-padding` | `8px` |
| `--ui-table-vertical-padding` | `8px` |
| `--ui-table-padding` | `8px` |
| `--ui-table-row-gap` | `0` |
| `--ui-table-column-gap` | `0` |
| `--ui-table-rounded-radius` | `var(--ui-border-radius-normal, 4px)` |
| `--ui-table-box-style` | `var(--ui-border-style, solid)` |
| `--ui-table-box-width` | `var(--ui-border-width, 1px)` |
| `--ui-table-box-color` | 30% table foreground mixed with 70% table background |
| `--ui-table-striped-background` | 10% table foreground mixed with 90% table background |
| `--ui-table-striped-foreground` | Table foreground |
| `--ui-table-hover-background` | 20% table foreground mixed with 80% table background |
| `--ui-table-hover-foreground` | Table foreground |
| `--ui-table-selected-background` | 15% selected foreground mixed with 85% selected background |
| `--ui-table-selected-foreground` | Table foreground |
| `--ui-table-dragging-row-background` | `var(--ui-background, hsl(0 0% 97% / 1))` |
| `--ui-table-dragging-row-foreground` | `var(--ui-foreground, hsl(0 0% 18% / 1))` |
| `--ui-table-dragging-row-shadow` | `0 1px 4px 0 rgb(from var(--ui-module-foreground, hsl(0 0% 18% / 1)) r g b / 50%)` |
| `--ui-table-dragging-row-border` | `unset` |
| `--ui-table-dragging-row-border-radius` | `var(--ui-border-radius-small, 3px)` |
| `--ui-table-dragging-row-opacity` | `1` |
| `--ui-table-dragging-header-background` | Header background |
| `--ui-table-dragging-header-foreground` | Header foreground |
| `--ui-table-dragging-header-shadow` | `0 1px 4px 0 rgb(from var(--ui-module-foreground, hsl(0 0% 18% / 1)) r g b / 50%)` |
| `--ui-table-dragging-header-border` | `unset` |
| `--ui-table-dragging-header-border-radius` | `var(--ui-border-radius-small, 3px)` |
| `--ui-table-dragging-header-opacity` | `1` |
| `--ui-table-animation-duration` | `var(--ui-animation-duration-default, 200ms)` |
| `--ui-table-dropzone-background` | `var(--ui-background, hsl(0 0% 97% / 1))` |
| `--ui-table-dropzone-border` | `unset` |
| `--ui-table-column-bar-highlight` | 15% selected foreground mixed with 85% selected background |
| `--ui-table-column-bar-selected` | 35% selected foreground mixed with 65% selected background |
| `--ui-table-focus-color-row` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-table-focus-color-header` | Table foreground |
| `--ui-table-focus-style` | `solid` |
| `--ui-table-focus-size` | `3px` |
| `--ui-table-disabled-filter` | `brightness(0.75)` |

Column widths can alternatively be configured with the `--ui-table-template-columns` custom property. Its default is `unset`; when `column-widths` is absent, the component falls back to equal `1fr` columns unless this property supplies a grid template. Respect the CSS grid syntax when setting either width configuration.
