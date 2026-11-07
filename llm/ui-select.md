# `<ui-select>`

`<ui-select>` is a form-associated select component. It supports a custom single-select picker, a native browser select picker, and a custom multi-select with checkboxes and removable tags.

## Import

```js
import ".../ui-web/ui-select.js";
```

## Usage

Provide option elements as children. Each option needs a non-empty `value` attribute in the custom picker modes; its text content is displayed as the option label.

```html
<ui-select name="country" placeholder="Choose a country">
  <div value="ca">Canada</div>
  <div value="jp">Japan</div>
  <div value="gb">United Kingdom</div>
</ui-select>
```

The `placeholder` is displayed when there is no selection. `slot="start"` and `slot="end"` children are reserved for adornments and are not treated as options.

### Single selection

The default mode uses a custom dialog picker. Click the control or press `Space`/`Enter` to open it. Select an option by clicking it or highlighting it and pressing `Space`/`Enter`. `Escape` closes the picker, and clicking outside closes it.

### Native browser picker

Add `default-select` to use an internal native `<select>` instead of the custom picker. This can use the platform's picker UI, which may be preferable on mobile.

```html
<ui-select default-select placeholder="Choose a country">
  <div value="ca">Canada</div>
  <div value="jp">Japan</div>
</ui-select>
```

In this mode, child options without a non-empty `value` use their text as the native option value. `search` does not work with `default-select`. Do not combine `default-select` with `checkbox`: the current implementation does not activate either mode when both are present.

### Multiple selection

Add `checkbox` to enable multiple selection. The selected values are returned as a comma-separated string, in selection order. Option values therefore should not contain commas and should be unique.

```html
<ui-select checkbox placeholder="Choose any">
  <div value="html">HTML</div>
  <div value="css">CSS</div>
  <div value="js">JavaScript</div>
</ui-select>
```

Selected options appear as removable tags. The component remains open while options are selected.

### Search

Add `search` to show a text input that filters custom picker options by a case-insensitive substring match against their text. It can be combined with the default single-select or `checkbox` mode, but not with `default-select`.

```html
<ui-select search placeholder="Search countries">
  <div value="ca">Canada</div>
  <div value="jp">Japan</div>
</ui-select>
```

The `placeholder` text is also used as the search input's placeholder.

## Attributes

| Attribute | Effect |
| --- | --- |
| `value` | Selected value. Use one value for single selection or a comma-separated list for `checkbox` mode. |
| `placeholder` | Text shown when there is no selection; also used as the search input placeholder when `search` is enabled. |
| `checkbox` | Enables custom multiple selection with checkboxes and removable selected tags. |
| `default-select` | Uses an internal native `<select>` for the picker instead of the custom dialog. |
| `search` | Adds search filtering to the custom picker. Not supported with `default-select`. |
| `clear` | Shows a clear control when a value or search text is present. Clearing removes the selection and, in search mode, clears the search text. |
| `disabled` | Disables user interaction, including when disabled through a containing form fieldset. |
| `loading` | Shows the loading message/spinner in the custom picker. |
| `loading-message` | Custom text for the loading message; defaults to `loading`. |
| `missing-message` | Custom text shown when no options are available or search finds none; defaults to `nothing found`. |
| `name` | Form field name used when submitting this form-associated control. |

`value` and `loading` are observed attributes. The mode/presentation attributes (`checkbox`, `default-select`, `search`, `clear`, and `placeholder`) are read when the component connects; set them in markup before connection rather than changing them dynamically. Changes to option child insertion/removal are observed, but changing an existing option's text or `value` attribute is not.

## Properties and methods

| API | Behavior |
| --- | --- |
| `value` | Gets or sets the current selection. The getter returns a string; use a comma-separated string for multiple selection. |
| `search` | Gets or sets the search text when `search` is enabled. Returns an empty string when search is not active. |
| `showPicker()` | Opens the custom picker. In `default-select` mode, the current implementation does not open the native picker through this method. |
| `hidePicker()` | Closes the custom picker. |

```js
const select = document.querySelector("ui-select");

select.value = "ca";
console.log(select.value);
select.showPicker();

const multiSelect = document.querySelector("ui-select[checkbox]");
multiSelect.value = "html,js";
```

In custom `checkbox` mode, the `value` list is kept in selection order. Setting `search` filters the options and opens the custom picker.

## Events

Listen on the `<ui-select>` element:

```js
select.addEventListener("change", (event) => {
  console.log(event.target.value);
});

select.addEventListener("input", (event) => {
  console.log(event.target.search);
});
```

- `change` bubbles and is composed. It fires when a user selection changes, including clearing a selection. It is not fired if the value is unchanged.
- `input` is used for search text changes when `search` is enabled; it is not emitted for ordinary selection changes.
- Assigning the `value` property updates the displayed selection but does not dispatch `change` or update the form-associated value. Programmatic property updates therefore do not synchronize form submission in the current implementation.

## Forms

The component is form-associated. Set `name` on the host to include its selection in form submission. Single-select mode submits one value; checkbox mode submits the comma-separated selected values. The component supports disabled-state handling, form reset, and state restoration.

## CSS customization

Set these custom properties on `<ui-select>` or an ancestor:

| Variable | Default |
| --- | --- |
| `--ui-select-highlight-background` | `color-mix(in hsl, var(--ui-foreground, hsl(0 0% 18% / 1)) 20%, var(--ui-background, hsl(0 0% 97% / 1)) 80%)` |
| `--ui-select-highlight-foreground` | `var(--ui-foreground, hsl(0 0% 18% / 1))` |
| `--ui-select-selected-background` | `var(--ui-selected-background, hsl(215 50% 90% / 1))` |
| `--ui-select-selected-foreground` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-select-font-size` | `var(--ui-font-size-normal, 16px)` |
| `--ui-select-font-weight` | `var(--ui-font-weight-normal, normal)` |
| `--ui-select-padding` | `4px 8px` |
| `--ui-select-bar-width` | `4px` |
| `--ui-select-bar-color` | `transparent` |
| `--ui-select-selected-bar-color` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-select-tag-background` | `var(--ui-default-background, hsl(0 0% 90% / 1))` |
| `--ui-select-tag-foreground` | `var(--ui-default-foreground, hsl(0 0% 30% / 1))` |
| `--ui-select-tag-delete-background` | `var(--ui-danger-background, hsl(0 50% 90% / 1))` |
| `--ui-select-tag-delete-foreground` | `var(--ui-danger-foreground, hsl(0 50% 30% / 1))` |
| `--ui-select-tag-font-size` | `var(--ui-font-size-small, 12px)` |
| `--ui-select-tag-padding` | `2px 4px` |
| `--ui-select-tag-gap` | `4px` |
| `--ui-select-tag-delete-size` | `16px` |
| `--ui-select-checkbox-accent-color` | `var(--ui-selected-foreground, hsl(215 50% 30% / 1))` |
| `--ui-select-checkbox-size` | `16px` |
| `--ui-select-checkbox-gap` | `8px` |
| `--ui-select-gap` | `8px` |
| `--ui-select-shift` | `2px` |

The `bar` variables control the option's leading indicator, while the tag and checkbox variables apply only in `checkbox` mode. `--ui-select-gap` sets the vertical separation between the control and custom picker; `--ui-select-shift` adjusts the picker's inline offsets.
