# `<ui-pagination>`

`<ui-pagination>` displays page controls for a paginated list. It provides first, previous, next, and last controls, plus a window of page-number buttons. The parent application handles the selected-page event and loads the corresponding content.

## Import

```js
import ".../ui-web/ui-pagination.js";
```

## Usage

```html
<ui-pagination total="20" show="5" page="3"></ui-pagination>
```

Listen for `selected` to respond to page changes:

```js
const pagination = document.querySelector("ui-pagination");

pagination.addEventListener("selected", (event) => {
  const { page, total } = event.target;
  loadPage(page, total);
});
```

The event bubbles from the `<ui-pagination>` element. It is emitted when the component updates its page display after a change to `page`, `show`, or `total`; this includes programmatic updates, not only user clicks. The event has no custom `detail` payload.

## Attributes and properties

| Attribute / property | Type | Default | Description |
| --- | --- | --- | --- |
| `total` | integer | `0` | Total number of selectable pages. A total of `0` hides the page-number controls. |
| `show` | integer | `1` | Maximum number of page numbers requested in the list. |
| `page` | integer | First page is displayed by default | Current page. Valid page values are from `1` through `total`. |
| `numbers-only` | boolean attribute | absent | Shows first/last navigation arrows with page numbers, without the endpoint numbers and ellipsis controls. |
| `small` | boolean attribute | absent | Uses the small text and icon sizes. |
| `large` | boolean attribute | absent | Uses the large text and icon sizes. |
| `boxed` | boolean attribute | absent | Uses joined, bordered button styling. |

`total`, `show`, and `page` are also available as JavaScript properties:

```js
pagination.total = 50;
pagination.show = 7;
pagination.page = 4;

console.log(pagination.page, pagination.total);
```

If `page` is omitted, the component displays page 1 when there are pages. The `page` getter returns `0` before the component has initialized a current page or when no page is available. The component does not validate or clamp malformed or out-of-range values; supply non-negative integer totals, a positive `show` count, and a `page` in range.

## Display modes

In the default mode, the component may show the first and last page numbers separated from the current page window by ellipses. With `numbers-only`, it omits those endpoint and ellipsis indicators and displays only a window of page numbers plus first/previous/next/last arrow controls.

```html
<ui-pagination total="20" show="5"></ui-pagination>
<ui-pagination total="20" show="5" numbers-only></ui-pagination>
```

The selected page is visually highlighted. `small` and `large` adjust the pagination text size and arrow icon size; `boxed` changes the button borders and joins adjacent controls.

## Styling

Set these CSS custom properties on the component or an ancestor:

| Variable | Default |
| --- | --- |
| `--ui-pagination-small-font-size` | `var(--ui-font-size-small, 12px)` |
| `--ui-pagination-normal-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-pagination-large-font-size` | `var(--ui-font-size-large, 24px)` |
| `--ui-pagination-gap` | `4px` |
| `--ui-pagination-background` | `transparent` |
| `--ui-pagination-foreground` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-pagination-selected-background` | `var(--ui-selected-background, hsl(215 50% 90% / 1))` |
| `--ui-pagination-selected-foreground` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-pagination-hover-background` | `var(--ui-default-background, hsl(0 0% 90% / 1))` |
| `--ui-pagination-hover-foreground` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-pagination-button-radius` | `var(--ui-border-radius-normal, 4px)` |
| `--ui-pagination-boxed-radius` | `var(--ui-border-radius-normal, 4px)` |
| `--ui-pagination-boxed-color` | `var(--ui-midground, hsl(0 0% 90% / 1))` |
| `--ui-pagination-boxed-border-width` | `2px` |
| `--ui-pagination-boxed-bar-width` | `1px` |
| `--ui-pagination-small-icon-height` | `12px` |
| `--ui-pagination-normal-icon-height` | `16px` |
| `--ui-pagination-large-icon-height` | `36px` |
| `--ui-pagination-stroke-width` | `2` |

```css
ui-pagination {
  --ui-pagination-gap: 8px;
  --ui-pagination-selected-background: #dbeafe;
  --ui-pagination-boxed-color: #94a3b8;
}
```
