# `UiOverlay` / `<ui-overlay>`

`UiOverlay` is a static JavaScript API for temporarily displaying an existing `HTMLElement` above the page. It can position the element at viewport coordinates or around an anchor element, adjusts placement to fit the viewport by default, and supports automatic dismissal and click-through behavior.

## Import

```js
import UiOverlay from ".../ui-web/ui-overlay.js";
```

The module registers `<ui-overlay>`. Normally use `UiOverlay.open()` and `UiOverlay.close()` rather than creating the custom element directly.

## Open and close

```js
const menu = document.createElement("ui-popup-menu");
menu.innerHTML = `
  <ui-menu-button>Open</ui-menu-button>
  <ui-menu-button>Settings</ui-menu-button>
`;

UiOverlay.open(menu, {
  anchorElement: trigger,
  location: "bottom-left",
  gap: 8
});

// Close it manually when needed:
UiOverlay.close();
```

Only one overlay can be open at a time; attempting to open a second throws an error. `close()` is safe to call when no overlay is open. The element passed to `open()` is moved into the overlay and removed from its former parent. Closing removes it from the overlay; it is not automatically restored to its original position.

## Options

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `showX` | `number` | `0` | Viewport X coordinate for point-based positioning. |
| `showY` | `number` | `0` | Viewport Y coordinate for point-based positioning. |
| `verticalAlignment` | `"top" \| "center" \| "bottom"` | `"bottom"` | Vertical position relative to the coordinate. |
| `horizontalAlignment` | `"left" \| "center" \| "right"` | `"right"` | Horizontal position relative to the coordinate. |
| `anchorElement` | `HTMLElement` | — | Element to position the overlay content around. |
| `location` | string | `"bottom-left"` | Anchor placement. Supported values: `top-left`, `top-center`, `top-right`, `bottom-left`, `bottom-center`, `bottom-right`, `left-top`, `left-center`, `left-bottom`, `right-top`, `right-center`, `right-bottom`. |
| `gap` | `number` | `0` | Pixel gap between the content and its anchor. |
| `fixed` | `boolean` | `false` | When `false`, placement can flip to fit within the viewport; when `true`, it keeps the requested alignment/location. |
| `locked` | `boolean` | `false` | Prevents Escape and outside interactions from closing the overlay. Call `close()` explicitly. |
| `clickThrough` | `boolean` | `false` | For an outside click or context-menu event, closes the overlay and redispatches that event to the element underneath. |
| `activeUiDialog` | `UiDialog` | — | Places the overlay inside the active dialog's shadow tree. |
| `activeUiDrawer` | `UiDrawer` | — | Places the overlay inside the active drawer's shadow tree. |

With `anchorElement`, `location` controls placement and `showX`, `showY`, and alignment options are not used. Without an anchor, the coordinates and alignments determine placement. When `fixed` is false, the component may change the requested location or alignment to avoid viewport edges.

## Dismissal and interaction

- Pressing Escape closes an unlocked overlay.
- Clicking outside the displayed element closes it unless locked.
- With `clickThrough: true`, an outside click is replayed as a click on the element beneath the pointer. An outside `contextmenu` event is similarly replayed.
- Page scrolling is locked while the overlay is open. Wheel and touch-move events outside the displayed content are prevented.
- Scroll-related keys are prevented from scrolling the page unless the event originates in an input, textarea, or editable element.

## Events

Events are dispatched on `document`:

| Event | `detail` |
| --- | --- |
| `overlayopen` | None |
| `overlayclose` | None |
| `overlayrelocation` | `{ oldLocation, newLocation }` |
| `overlayrealignment` | `{ oldVerticalAlignment, oldHorizontalAlignment, newVerticalAlignment, newHorizontalAlignment }` |
| `overlaymove` | `{ oldX, oldY, newX, newY }` |

Relocation, realignment, and move events are emitted when viewport fitting or resizing changes the placement.
