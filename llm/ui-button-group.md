# `<ui-button-group>`

A single-selection group that presents its direct child elements as buttons. It supports horizontal or vertical layouts, selection through click or the `selected` property, and optional per-button status colors.

## Import

```js
import ".../ui-web/ui-button-group.js";
```

The component is registered as `<ui-button-group>`. It creates an open shadow root with a `tablist` container and generates a native button for each direct child element. Each child is assigned to its generated button through a slot. Use simple elements such as `<div>` or other elements as the group's direct children.

## Basic usage

```html
<ui-button-group>
  <div>Day</div>
  <div>Week</div>
  <div>Month</div>
</ui-button-group>
```

Click an option to select it. Only one option can be selected at a time; the group starts with no selection unless the `selected` property is set.

## Properties

| Property | Behavior |
| --- | --- |
| `selected` | Gets or sets the selected option's 1-based index. `0` means no option selected. |
| `selectedElement` | Read-only; returns the selected original child element, or `null` when no option is selected. |

```js
const group = document.querySelector("ui-button-group");

group.selected = 2;
console.log(group.selected); // 2
console.log(group.selectedElement); // the second direct child element
group.selected = 0; // clear selection
```

The properties track position in the current direct-child list, not a child's `value` attribute. Read any application-specific value from `selectedElement`:

```html
<ui-button-group id="period">
  <div value="day">Day</div>
  <div value="week">Week</div>
</ui-button-group>
```

```js
const group = document.querySelector("#period");
const value = group.selectedElement?.getAttribute("value");
```

The group observes direct child-list changes and rebuilds its generated buttons when children are added or removed. It does not observe child attribute changes.

## Attributes

Boolean attributes can be combined unless otherwise noted.

| Attribute | Effect |
| --- | --- |
| `vertical` | Stacks options vertically and uses up/down arrow navigation. Without it, layout is horizontal and left/right arrows are used. |
| `bar` | Adds dividers between adjacent options. |
| `outline` | Adds a border around the group/options, along with dividers. |
| `boxed` | Wraps the buttons in a bordered, padded container with gaps between the buttons. |
| `pill` | Rounds the ends of the group; can combine with `boxed`. |
| `blank` | Makes unselected buttons transparent at rest, showing their background on hover/press. |
| `small` | Uses small text, padding, icon dimensions, and corner radius. |
| `large` | Uses large text, padding, icon dimensions, and corner radius. |
| `fill-width` | Stretches the group and lets buttons grow to fill the available width. |
| `success` | Applies success colors to the group buttons. |
| `warning` | Applies warning colors to the group buttons. |
| `danger` | Applies danger colors to the group buttons. |
| `info` | Applies info colors to the group buttons. |
| `highlight` | Applies highlight colors to the group buttons. |

The per-option `success`, `warning`, `danger`, `info`, and `highlight` attributes can be placed on an individual direct child instead of the group to color only that option. `disabled` on a direct child disables its generated button. These child attributes are copied when the buttons are built; see the dynamic-update note below.

The generated selected button uses the selected-state colors and outline. Setting a group-wide status attribute colors the options, while setting a status attribute on a child applies that color to the individual option.

## Icons

Place icon elements directly inside the group and give each `slot="icon"`:

```html
<ui-button-group outline>
  <ui-icon-check slot="icon"></ui-icon-check>
  <ui-icon-cross slot="icon"></ui-icon-cross>
  <ui-icon-question slot="icon"></ui-icon-question>
</ui-button-group>
```

Each direct child, including an icon, becomes one option. The source sizes slotted icon elements using the icon-size variables below. Add accessible names or accompanying text for icon-only options so their purpose is clear to assistive technology.

## CSS customization

Set CSS custom properties on `<ui-button-group>` or an ancestor, such as `:root`. The group and generated buttons consume these variables in the shadow styles.

### Colors

| Variable | Default / fallback |
| --- | --- |
| `--ui-button-group-default-background` | `var(--ui-default-background, hsl(0 0% 90% / 1))` |
| `--ui-button-group-default-foreground` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-button-group-selected-background` | `var(--ui-selected-background, hsl(215 50% 90% / 1))` |
| `--ui-button-group-selected-foreground` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-button-group-success-background` | `var(--ui-success-background, hsl(104 50% 90% / 1))` |
| `--ui-button-group-success-foreground` | `var(--ui-success-foreground, hsl(104 50% 30% / 1))` |
| `--ui-button-group-warning-background` | `var(--ui-warning-background, hsl(45 50% 90% / 1))` |
| `--ui-button-group-warning-foreground` | `var(--ui-warning-foreground, hsl(45 50% 30% / 1))` |
| `--ui-button-group-danger-background` | `var(--ui-danger-background, hsl(0 50% 90% / 1))` |
| `--ui-button-group-danger-foreground` | `var(--ui-danger-foreground, hsl(0 50% 30% / 1))` |
| `--ui-button-group-info-background` | `var(--ui-info-background, hsl(190 50% 90% / 1))` |
| `--ui-button-group-info-foreground` | `var(--ui-info-foreground, hsl(190 90% 30% / 1))` |
| `--ui-button-group-highlight-background` | `var(--ui-highlight-background, hsl(302 50% 90% / 1))` |
| `--ui-button-group-highlight-foreground` | `var(--ui-highlight-foreground, hsl(302 50% 30% / 1))` |
| `--ui-button-group-background` | Current button background; defaults to the default-state background. |
| `--ui-button-group-foreground` | Current button foreground; defaults to the default-state foreground. |

### Text, spacing, borders, and box

| Variable | Default / fallback |
| --- | --- |
| `--ui-button-group-small-font-size` | `var(--ui-font-size-small, 12px)` |
| `--ui-button-group-normal-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-button-group-large-font-size` | `var(--ui-font-size-large, 24px)` |
| `--ui-button-group-font-weight` | `var(--ui-font-weight-bold, 600)` |
| `--ui-button-group-small-padding` | `4px 8px` |
| `--ui-button-group-normal-padding` | `8px 16px` |
| `--ui-button-group-large-padding` | `12px 24px` |
| `--ui-button-group-small-icon-size` | `12px` |
| `--ui-button-group-normal-icon-size` | `16px` |
| `--ui-button-group-large-icon-size` | `24px` |
| `--ui-button-group-small-icon-padding` | `4px 0` |
| `--ui-button-group-normal-icon-padding` | `4px 0` |
| `--ui-button-group-large-icon-padding` | `4px 0` |
| `--ui-button-group-border-small-radius` | `var(--ui-border-radius-small, 3px)` |
| `--ui-button-group-border-normal-radius` | `var(--ui-border-radius-normal, 4px)` |
| `--ui-button-group-border-large-radius` | `var(--ui-border-radius-large, 6px)` |
| `--ui-button-group-border-style` | `solid` |
| `--ui-button-group-border-width` | `var(--ui-border-width, 1px)` |
| `--ui-button-group-border-color` | `var(--ui-button-group-foreground)` |
| `--ui-button-group-outline-style` | `solid` |
| `--ui-button-group-outline-width` | `2px` |
| `--ui-button-group-box-padding` | `4px` |
| `--ui-button-group-box-gap` | `4px` |
| `--ui-button-group-box-small-radius` | `5px` |
| `--ui-button-group-box-normal-radius` | `6px` |
| `--ui-button-group-box-large-radius` | `8px` |
| `--ui-button-group-box-shadow` | `var(--ui-border-box-shadow, none)` |

### Current-style variables

These variables set the current computed style directly:

| Variable | Default / fallback |
| --- | --- |
| `--ui-button-group-font-size` | `var(--ui-button-group-normal-font-size, var(--ui-font-size-normal, 16px))` |
| `--ui-button-group-padding` | `var(--ui-button-group-normal-padding, 8px 16px)` |
| `--ui-button-group-icon-size` | `var(--ui-button-group-normal-icon-size, 16px)` |
| `--ui-button-group-icon-padding` | Not explicitly initialized for normal size; set it directly if icon padding needs adjustment. |
| `--ui-button-group-border-radius` | `var(--ui-button-group-border-normal-radius, var(--ui-border-radius-normal, 4px))` |
| `--ui-button-group-box-radius` | `var(--ui-button-group-box-normal-radius, 6px)` |

Example:

```html
<ui-button-group
  outline
  style="
    --ui-button-group-background: #eee;
    --ui-button-group-foreground: #222;
    --ui-button-group-padding: 0.5rem 1rem;
    --ui-button-group-border-color: #222;">
  <div>First</div>
  <div>Second</div>
</ui-button-group>
```

## Keyboard and accessibility notes

- The shadow DOM exposes a `role="tablist"` container with generated `role="tab"` buttons. It sets `aria-orientation` to `horizontal` or `vertical` when connected.
- Arrow keys move focus to the next/previous enabled button in the current orientation. `Home` and `End` move focus to the first/last enabled button. Navigation does not wrap at the ends.
- Click selection sets `aria-selected="true"` on the selected generated button and removes it from the others.
- Focus movement by keyboard does not itself select an option; click selects. The implementation does not emit a custom selection-change event.
- Add suitable accessible names to icon-only items. Consider whether tablist/tab semantics match the way the group is used in your interface.
- Forced-colors and increased-contrast modes strengthen borders; reduced-motion preferences disable button transitions.

## Implementation caveats

- The `selected` property is a 1-based index, while the component does not read an initial `selected` attribute or a `selected` attribute on its child elements.
- Setting `selected` changes the visual selection but does not update `aria-selected`; that ARIA state is only set and cleared by click handling.
- Changing a child's `disabled` or status attributes after insertion does not update its generated button. The component rebuilds on child-list changes, not attribute changes.
- Changing `vertical` after the component is connected updates layout styles, but the `aria-orientation` value is only set in `connectedCallback`; reconnect the element for the value to be recalculated.
