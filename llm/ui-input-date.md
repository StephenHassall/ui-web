# `<ui-input-date>`

`<ui-input-date>` is a form-associated date input with a popup calendar. It can optionally provide time selection, use the browser's native date control, and format its displayed date using a locale.

## Import

```js
import ".../ui-web/ui-input-date.js";
```

## Basic usage

```html
<form>
  <label for="meeting-date">Meeting date</label>
  <ui-input-date
    id="meeting-date"
    name="meetingDate"
    value="2026-10-29"
    placeholder="Select a date...">
  </ui-input-date>
  <button type="submit">Save</button>
</form>
```

Click the input to open the calendar. The displayed date is formatted for the selected locale, while the component value uses an ISO-like local date string:

- Date only: `YYYY-MM-DD`
- With `time`: `YYYY-MM-DDTHH:MM:SS`

The value has no timezone offset; date and time are interpreted in the user's local timezone. The picker supports dates from 1800 through 2500.

## Attributes

| Attribute | Effect |
| --- | --- |
| `value` | Sets the initial/current value using one of the formats above. |
| `time` | Adds hour and minute selection and changes the value format to include time. Selecting a date does not close the picker in this mode. |
| `seconds` | Shows seconds in the time selector; use with `time`. |
| `region` | Sets the locale used for the displayed date and calendar labels. Accepts an `Intl.DateTimeFormat` locale identifier such as `fr-FR`. |
| `default-input` | Uses the browser's native `<input type="date">`, or `<input type="datetime-local">` when `time` is also present. |
| `today` | Highlights today's date in the custom calendar. |
| `start-day` | Chooses the first weekday in the calendar: `sun`, `mon`, `tue`, `web`, `thu`, `fri`, or `sat`. |
| `left` | Aligns the popup to the left edge of the input. |
| `right` | Aligns the popup to the right edge of the input. |
| `placeholder` | Text shown when no date is set. |
| `disabled` | Prevents interaction with the picker. |
| `valid` / `invalid` | Applies shared input presentation styles; validity is not automatically determined. |

With neither `left` nor `right`, the custom popup is centered below the input. The component includes `start` and `end` slots for icons or other supplementary content.

## Value and formatting API

The `value` property gets or sets the component's date/time string:

```js
const dateInput = document.querySelector("ui-input-date");
dateInput.value = "2026-10-29";
console.log(dateInput.value);
```

The setter accepts a date string (`YYYY-MM-DD`) or a date-time string (`YYYY-MM-DDTHH:MM:SS`). Invalid non-empty strings are ignored. Assign an empty string to clear the current selection.

`formatOptions` exposes the `Intl.DateTimeFormat` options used for the human-readable display. Modify properties on the returned object and the component refreshes its display:

```js
dateInput.formatOptions.weekday = "long";
dateInput.formatOptions.year = "numeric";
dateInput.formatOptions.month = "long";
dateInput.formatOptions.day = "numeric";
```

The component is form-associated. Set a `name` attribute to identify its submitted form value. It supports disabled fieldsets, browser form state restoration, and form reset.

## Picker methods

| Method | Behavior |
| --- | --- |
| `showPicker()` | Opens the custom calendar or invokes the browser picker in `default-input` mode. Does nothing if already open or disabled. |
| `hidePicker()` | Closes the custom calendar. In `default-input` mode, it removes and re-adds the internal native input as a workaround; the native picker itself has no standard close method. |

The custom picker closes when the user clicks outside it or presses Escape. When `time` is enabled, selecting a date leaves it open so the time can also be chosen.

## Keyboard interaction

The custom calendar supports keyboard navigation among days, months, years, and time values. Space or Enter opens the picker. While it is open, Escape closes it. You can enter a day offset by typing `+` or `-`, followed by a number of days, then pressing Enter.

## Events

The component dispatches bubbling, composed events from the host:

- `input` fires when a date or time value changes in the custom picker.
- `change` fires when the custom picker closes after the value changed. In native input mode, it is dispatched when the native input commits a change.

```js
dateInput.addEventListener("input", (event) => {
  console.log("Current value:", event.target.value);
});

dateInput.addEventListener("change", (event) => {
  console.log("Committed value:", event.target.value);
});
```

## CSS customization

Set these component-specific custom properties on `<ui-input-date>` or an ancestor:

| Variable | Default |
| --- | --- |
| `--ui-input-date-dialog-height` | `300px` |
| `--ui-input-date-dialog-date-width` | `350px` |
| `--ui-input-date-dialog-time-width` | `150px` |
| `--ui-input-date-dialog-font-size-small` | `var(--ui-font-size-small, 12px)` |
| `--ui-input-date-dialog-font-size-normal` | `var(--ui-font-size-normal, 16px)` |
| `--ui-input-date-dialog-font-size-large` | `var(--ui-font-size-large, 24px)` |
| `--ui-input-date-dialog-button-radius` | `var(--ui-border-radius-normal, 4px)` |
| `--ui-input-date-gap` | `8px` |
| `--ui-input-date-shift` | `2px` |

The time column is added to the date width when `time` is enabled. Shared input variables (such as `--ui-input-background`, `--ui-input-foreground`, and `--ui-input-border-color`) also style the control; see the common inputs reference for those options.

```css
ui-input-date {
  --ui-input-date-dialog-height: 360px;
  --ui-input-date-dialog-date-width: 400px;
  --ui-input-date-dialog-time-width: 180px;
  --ui-input-date-gap: 12px;
}
```
