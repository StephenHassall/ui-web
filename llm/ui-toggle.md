# `<ui-toggle>`

`<ui-toggle>` is a form-associated custom element that presents an on/off switch. Import its component module before using it:

```js
import ".../ui-web/ui-toggle.js";
```

```html
<ui-toggle></ui-toggle>
<ui-toggle checked></ui-toggle>
```

## State and interaction

- The initial state is off. Add the boolean `checked` attribute to initialize it on.
- Clicking the switch toggles its state. When focused, **Space** or **Enter** also toggles it.
- User interaction dispatches a bubbling, composed `change` event after updating the state.
- `checked` is the boolean state property. Assigning to it updates the state and dispatches `change`.
- `value` is a string property that returns `"true"` or `"false"`. Setting it to the case-insensitive string `"true"` turns the switch on; any other nonempty string turns it off. An empty/falsy value turns it off.
- Setting the `checked` or `value` HTML attribute updates state but does not dispatch `change`.
- The host's `aria-checked` attribute is updated to `"true"` or `"false"` as state changes.

```js
const toggle = document.querySelector("ui-toggle");

toggle.addEventListener("change", (event) => {
  console.log(event.target.checked);
});

toggle.checked = true;
console.log(toggle.value); // "true"
```

## Form behavior

The component is form-associated. Its form value is the state converted to a string (`"true"` or `"false"`). It participates in form reset, which returns it to off, and form state restoration. When disabled by a form or disabled fieldset, it is made non-interactive.

## Attributes

| Attribute | Purpose |
|---|---|
| `checked` | Initialize/set the on state; presence means on. |
| `value` | Set state from the string `"true"` (case-insensitive); other values mean off. |
| `disabled` | Prevent user interaction. |
| `outline` | Add a border around the switch. |
| `square` | Use a square-cornered track and thumb instead of the default pill shape. |
| `icons-off` | Hide the on/off icons. |
| `icons-inside` | Place the icons inside the moving thumb. |
| `reverse` | Reverse the switch's left-to-right direction. |

`outline`, `square`, `icons-off`, `icons-inside`, and `reverse` are presence-based styling attributes. They can be combined.

## Custom icons

The default on and off icons can be replaced independently using the `on` and `off` slots:

```html
<ui-toggle>
  <ui-icon-plus slot="on"></ui-icon-plus>
  <ui-icon-ban slot="off"></ui-icon-ban>
</ui-toggle>
```

Use `icons-inside` to position the custom icons inside the thumb. Supplying content to one of these slots hides that state's built-in SVG icon.

## CSS customization

Set custom properties on the component or an ancestor:

| Property | Default / purpose |
|---|---|
| `--ui-toggle-on-background` | On track background; falls back to `--ui-success-background`, then `hsl(104 50% 90% / 1)`. |
| `--ui-toggle-on-foreground` | On thumb/icon color; falls back to `--ui-success-foreground`, then `hsl(104 50% 30% / 1)`. |
| `--ui-toggle-off-background` | Off track background; falls back to `--ui-default-background`, then `hsl(0 0% 90% / 1)`. |
| `--ui-toggle-off-foreground` | Off thumb/icon color; falls back to `--ui-default-foreground`, then `hsl(0 0% 30% / 1)`. |
| `--ui-toggle-size` | Overall height basis; `24px`. |
| `--ui-toggle-padding` | Inset between track edge and thumb/icons; `3px`. |
| `--ui-toggle-gap` | Additional gap between the two state areas; `0`. |
| `--ui-toggle-border-style` | Border style when `outline` is present; `solid`. |
| `--ui-toggle-border-width` | Border width when `outline` is present; falls back to `--ui-border-width`, then `1px`. |
| `--ui-toggle-square-radius` | Corner radius when `square` is present; falls back to `--ui-border-radius-normal`, then `4px`. |
| `--ui-toggle-disabled-background` | Disabled background color. Defaults to a `color-mix()` based on `--ui-foreground` and `--ui-background`. |
| `--ui-toggle-disabled-foreground` | Disabled thumb/icon color. Defaults to a `color-mix()` based on `--ui-foreground` and `--ui-background`. |

Example:

```css
:root {
  --ui-toggle-on-background: purple;
  --ui-toggle-on-foreground: white;
  --ui-toggle-size: 36px;
}
```

The component also applies a visible keyboard-focus outline and disables transitions when reduced motion is requested.
