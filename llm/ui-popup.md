# `<ui-popup>`

`<ui-popup>` displays a slotted dialog area when its trigger is clicked. It is commonly used with `<ui-popup-menu>` and other menu components to build nested menus, but its dialog slot can contain other content.

## Import

```js
import ".../ui-web/ui-popup.js";
```

## Usage

Provide one trigger and one dialog using the named slots:

```html
<ui-popup bottom>
  <ui-button slot="trigger">Open menu</ui-button>
  <ui-popup-menu slot="dialog">
    <ui-menu-button>First action</ui-menu-button>
    <ui-menu-button>Second action</ui-menu-button>
  </ui-popup-menu>
</ui-popup>
```

The trigger and dialog can be any elements. Clicking the trigger opens the dialog; clicking it again closes the dialog. A click outside the dialog also closes it. Clicks within the dialog stop propagation, so they do not close the popup by themselves.

Only one top-level popup is open at a time. Nested popups can be used to form submenus; opening a popup closes its sibling popups, and closing a popup also closes open descendant popups.

## Slots

| Slot | Purpose |
| --- | --- |
| `trigger` | Element that toggles the popup. The first assigned element receives a `selected` attribute while the popup is open; it is removed when closed. |
| `dialog` | Content shown inside the popup's native `<dialog>` element. The first assigned element receives an `open` attribute while shown; it is removed when closed. |

Use one element per slot. The component operates on the first assigned element in each slot.

## Positioning attributes

Choose one of `top`, `bottom`, `left`, or `right` to position the dialog relative to the trigger. Add `start` or `end` to align the dialog to an edge. Without `start` or `end`, the dialog is centered along the corresponding axis.

| Position | Default alignment | `start` / `end` alignment |
| --- | --- | --- |
| `top` | Centered horizontally above the trigger | Aligns the dialog's start/end edge horizontally |
| `bottom` | Centered horizontally below the trigger | Aligns the dialog's start/end edge horizontally |
| `left` | Centered vertically to the left of the trigger | Aligns the dialog's top/bottom edge vertically |
| `right` | Centered vertically to the right of the trigger | Aligns the dialog's top/bottom edge vertically |

```html
<ui-popup right start>
  <ui-menu-button slot="trigger">More options</ui-menu-button>
  <ui-popup-menu slot="dialog">
    <ui-menu-button>Rename</ui-menu-button>
    <ui-menu-button>Delete</ui-menu-button>
  </ui-popup-menu>
</ui-popup>
```

## JavaScript API

Call `close()` to close an open popup programmatically:

```js
document.querySelector("ui-popup").close();
```

Calling `close()` when the popup is already closed has no effect. The component does not dispatch custom events.

## Styling

The host is an `inline-grid` positioned relative to its trigger. The dialog is absolutely positioned with no border, padding, or outline, and uses a high `z-index`. Set dimensions and visual styling on the slotted dialog content, such as `<ui-popup-menu>`.

The spacing between trigger and dialog is controlled by the `--gap` custom property, which defaults to `0.5rem`:

```css
ui-popup {
  --gap: 0.75rem;
}
```

The dialog text color defaults to `var(--ui-module-foreground, hsl(0 0% 75% / 1))`. A popup inside `<ui-menu-group>` uses grid display. Popups nested in menu contexts also receive internal alignment adjustments for nested `<ui-popup-menu>` elements.
