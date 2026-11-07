# `<ui-tree>`

`<ui-tree>` renders nested items as a selectable, expandable tree. Import its module:

```js
import ".../ui-web/ui-tree.js";
```

Define each item as a direct child `<div>`; nested `<div>` elements become child items. The direct text nodes in each item form its label.

```html
<ui-tree>
  <div>
    Documents
    <div>Annual report</div>
    <div>
      Plans
      <div>Roadmap</div>
    </div>
  </div>
</ui-tree>
```

The tree observes item child-list, text, and attribute changes and updates the rendered rows. It is not a native `<ul role="tree">` structure; the component does not add tree ARIA roles.

## Expanding and selecting items

- Items with children are expandable and open by default. Add `closed` to an item to start it collapsed.
- Click an item's toggle arrow to expand/collapse it. `toggle-row` on the tree makes clicking anywhere on an expandable row toggle it.
- `hide-toggle` hides an item's toggle arrow. With `toggle-row`, the row can still toggle.
- `disable-toggle` keeps an item's children open and prevents toggling.
- Clicking a row selects it and focuses it. The selected row is marked internally; retrieve its original light-DOM item with `getCurrentElement()`.
- Keyboard navigation uses **ArrowUp** and **ArrowDown** to move among visible rows, **Home** and **End** to move to the first/last row, and **PageUp**/**PageDown** to move by a page. **Space**, **Enter**, or **NumpadEnter** toggles an expandable item; on a leaf, it selects the item.

```js
const tree = document.querySelector("ui-tree");
const currentItem = tree.getCurrentElement();

tree.addEventListener("selected", (event) => {
  console.log("Selected item:", event.detail.element);
});

tree.addEventListener("toggled", (event) => {
  console.log(event.detail.element, event.detail.open);
});
```

Both custom events bubble and are composed. `selected` has `detail.element`, the original item element. `toggled` has `detail.element` and `detail.open` (`true` when opened).

## Item and tree attributes

| Where | Attribute | Purpose |
|---|---|---|
| `<ui-tree>` | `toggle-row` | Toggle expandable items by clicking anywhere on their row. |
| `<ui-tree>` | `start` | Enable the leading `start` slot area. |
| `<ui-tree>` | `end` | Enable the trailing `end` slot area. |
| `<ui-tree>` | `line` | Draw vertical hierarchy lines. |
| `<ui-tree>` | `nub` | With `line`, add horizontal connector nubs. |
| `<ui-tree>` | `drag-highlight` | Highlight a drop target item/branch during drag and drop. |
| `<ui-tree>` | `drag-insert` | Show an insertion line while dragging over the tree. |
| `<ui-tree>` | `drag-open-delay` | Milliseconds to hover over a closed parent during a drag before it opens. The default is 1000 ms. |
| Item `<div>` | `closed` | Start an expandable item collapsed. |
| Item `<div>` | `hide-toggle` | Hide the item's expand/collapse control. |
| Item `<div>` | `disable-toggle` | Prevent toggling the item. |
| Item `<div>` | `draggable` | Make the item draggable. |
| Item `<div>` | `insertable` | Mark an item as a valid container/drop target, including an empty item. Items with children are insertable automatically. |
| Item `<div>` | `style` | Inline styles copied onto the rendered row's item content container. |

## Decorative slots

Slot definitions are children of `<ui-tree>` with a unique `name`; items refer to that name through matching attributes. The component clones these definitions into rendered rows.

| Slot | Item attributes | Area |
|---|---|---|
| `prefix` | `prefix`, `prefix-open`, `prefix-close` | Before the label. |
| `suffix` | `suffix`, `suffix-open`, `suffix-close` | After the label. |
| `start` | `start`, `start-open`, `start-close` | Leading edge; requires `start` on `<ui-tree>`. |
| `end` | `end`, `end-open`, `end-close` | Trailing edge; requires `end` on `<ui-tree>`. |
| `toggle-open` | — | Custom icon/control for an expanded item. |
| `toggle-close` | — | Custom icon/control for a collapsed item. |

For prefix/suffix/start/end, the base slot supplies the content for non-expandable items; the `*-open` and `*-close` variants are used for expandable items as their state changes. `start-text` and `end-text` replace the text inside their corresponding cloned start/end element for a particular item.

```html
<ui-tree start>
  <span slot="start" name="folder">Folder</span>
  <span slot="start-open" name="folder-open">Open</span>
  <span slot="start-close" name="folder-closed">Closed</span>

  <div start="folder" start-open="folder-open" start-close="folder-closed">
    Projects
    <div>Website</div>
  </div>
</ui-tree>
```

## Public JavaScript API

| API | Description |
|---|---|
| `tree.getCurrentElement()` | Returns the currently selected original item element, or `null` if none is selected. |
| `tree.toggleElement(element, open)` | Opens/closes an item (`open` is boolean); no-op if the item is unknown or already in the requested state. |
| `UiTree.isToggleOpen(element)` | Returns whether an item with children is open; returns `false` for unknown items and leaves. |
| `UiTree.isInsertable(element)` | Returns whether an item can accept content; returns `false` for unknown items. |
| `UiTree.getDragFromTree()` | Tree where the current drag originated, or `null` for a drag from outside a tree. |
| `UiTree.getDragFromElement()` | Original item being dragged, or `null` for a drag from outside a tree. |
| `UiTree.getDragToTree()` | Tree currently being dragged over, or `null` if there is no tree target. |
| `UiTree.getDragToElement()` | Original item corresponding to the current drop target, or `null` if there is no row target. For `drag-highlight`, it can return an ancestor container when the target is not insertable. |

The drag getters are static methods on the imported `UiTree` class:

```js
import { UiTree } from ".../ui-web/ui-tree.js";

tree.addEventListener("drop", () => {
  const sourceTree = UiTree.getDragFromTree();
  const sourceItem = UiTree.getDragFromElement();
  const targetTree = UiTree.getDragToTree();
  const targetItem = UiTree.getDragToElement();

  if (!sourceTree || !sourceItem || !targetTree || !targetItem) return;
  // Update the application's item data and DOM.
});
```

The component provides drag affordances and target lookup; application code must handle drop events and move/update the items. In `drag-insert` mode, insert after a non-insertable target or append to an insertable target. In `drag-highlight` mode, append to the returned container. Native drag-and-drop is not supported on Android/iPhone per the usage guide.

## CSS custom properties

| Property | Default / purpose |
|---|---|
| `--ui-tree-background` | Tree background; falls back to `--ui-background`, then `hsl(0 0% 97% / 1)`. |
| `--ui-tree-foreground` | General foreground; falls back to `--ui-foreground`, then `hsl(0 0% 18% / 1)`. |
| `--ui-tree-size` | Row and standard icon-area sizing; `24px`. |
| `--ui-tree-icon-size` | Icon sizing; `16px`. |
| `--ui-tree-start-width` | Width of the start area; `24px`. |
| `--ui-tree-hover-background` | Hover row background; defaults to a `color-mix()` of tree foreground/background. |
| `--ui-tree-hover-foreground` | Hover row text; defaults to tree foreground. |
| `--ui-tree-hover-border-radius` | Hover row radius; falls back to `--ui-border-radius-small`, then `3px`. |
| `--ui-tree-selected-background` | Selected row background; defaults to a `color-mix()` of selected foreground/background. |
| `--ui-tree-selected-foreground` | Selected row text; defaults to tree foreground. |
| `--ui-tree-selected-border-radius` | Selected row radius; falls back to `--ui-border-radius-small`, then `3px`. |
| `--ui-tree-focus-color` | Focus outline color; falls back to selected foreground. |
| `--ui-tree-focus-style` | Focus outline style; `solid`. |
| `--ui-tree-focus-size` | Focus outline width; `2px`. |
| `--ui-tree-focus-border-radius` | Focus outline radius; falls back to `--ui-border-radius-small`, then `3px`. |
| `--ui-tree-line-size` | Hierarchy line width; `2px`. |
| `--ui-tree-line-color` | Hierarchy line color; falls back to `--ui-midground`, then `hsl(0 0% 90% / 1)`. |
| `--ui-tree-line-radius` | Nub corner radius; `8px`. |
| `--ui-tree-row-padding-inline` | Inline row padding; `8px`. |
| `--ui-tree-row-padding-block` | Block row padding; `2px`. |
| `--ui-tree-item-gap` | Gap between row content; stylesheet fallback is `8px`. |
| `--ui-tree-item-start-gap` | Space before item content; `4px`. |
| `--ui-tree-item-end-gap` | Space after item content; `0`. |
| `--ui-tree-item-color` | Item label color; falls back to `--ui-default-foreground`, then `hsl(0 0% 30% / 1)`. |
| `--ui-tree-item-font-size` | Item label size; falls back to `--ui-font-size-normal`, then `16px`. |
| `--ui-tree-item-font-weight` | Item label weight; falls back to `--ui-font-weight-normal`, then `normal`. |
| `--ui-tree-toggle-color` | Default chevron color; falls back to `--ui-foreground`, then `hsl(0 0% 18% / 1)`. |
| `--ui-tree-drag-highlight-background` | Drag-highlight background; defaults to a `color-mix()` of tree foreground/background. |
| `--ui-tree-drag-highlight-border-radius` | Drag-highlight radius; falls back to `--ui-border-radius-small`, then `3px`. |
| `--ui-tree-drag-insert-color` | Insert-line color; falls back to selected foreground. |
| `--ui-tree-drag-insert-size` | Insert-line thickness; `2px`. |
| `--ui-tree-drag-insert-style` | Insert-line style; `solid`. |

The tree also has visible focus styling, forced-colors/high-contrast adjustments, and reduced-motion support.

## Documentation/source notes

- The CSS reference in the stylesheet comments and human CSS help lists `--ui-tree-item-gap` as `4px`, but the actual CSS declaration falls back to `8px` when the custom property is unset.
- The human usage help's End Slots section mistakenly tells you to use an item's `start` attribute. The implementation uses the item's `end` attribute to select an end slot.
