# ui-web: LLM guide index

`ui-web` is a collection of browser-native UI web components. It uses standard HTML, CSS, JavaScript, and Web Components APIs; it does not require a framework, TypeScript, Sass, or a component-specific build tool. The library is in beta and may contain bugs or other issues.

## Get started

Install the package from npm:

```sh
npm install @coderundebug/ui-web
```

Load the JavaScript module for each component you use. You do not need to load the entire library. For example:

```html
<!doctype html>
<html lang="en">
  <head>
    <script type="module" src="/node_modules/@coderundebug/ui-web/ui-button.js"></script>
  </head>
  <body>
    <ui-button>Press Me</ui-button>
  </body>
</html>
```

You can also import the needed components from a JavaScript module:

```js
import "/node_modules/@coderundebug/ui-web/ui-button.js";
import "/node_modules/@coderundebug/ui-web/ui-lozenge.js";
```

After the module loads, use the registered custom element in HTML. If JavaScript needs to call a component's methods or access its properties, wait until the browser has registered it:

```js
await customElements.whenDefined("ui-table");
// The <ui-table> custom element is now defined and can be used.
```

The package can also be downloaded from [the ui-web GitHub repository](https://github.com/StephenHassall/ui-web); its files need to be served from a location accessible to your pages. See the [human-facing getting-started page](../web-dev/ui/api-get-started.html) for the original installation and usage examples.

## Component guides

Use the guide for the specific component before writing markup or code. Each guide describes its API and implementation-specific behavior; where applicable it also documents slots, events, CSS variables, and caveats. Shared theme and CSS defaults are listed below.

The guides below follow the component order in `web-dev/side-bar/side-bar.html` and are grouped under headings for easier scanning. Only guides that currently exist in this folder are listed.

### Shared CSS

- [Standard CSS defaults](default-css.md)

### Components

- [Alert](ui-alert.md)
- [Badge](ui-badge.md)
- [Code](ui-code.md)
- [Dialog](ui-dialog.md)
- [Drawer](ui-drawer.md)
- [Group](ui-group.md)
- [Icons](ui-icon.md)
- [Lorem](ui-lorem.md)
- [Lozenge](ui-lozenge.md)
- [Overlay](ui-overlay.md)
- [Pagination](ui-pagination.md)
- [Pattern](ui-pattern.md)
- [Popup](ui-popup.md)
- [Split Layout](ui-split-layout.md)
- [Stepper](ui-stepper.md)
- [Tab](ui-tab.md)
- [Table](ui-table.md)
- [Theme](ui-theme.md)
- [Toast](ui-toast.md)
- [Tooltip](ui-tooltip.md)
- [Tree](ui-tree.md)

### Buttons

- [Button](ui-button.md)
- [Button Group](ui-button-group.md)
- [Link Button](ui-link-button.md)
- [Icon Button](ui-icon-button.md)
- [Icon Link Button](ui-icon-link-button.md)

### Charts

- [Bar Chart](ui-bar-chart.md)
- [Chart Legend](ui-chart-legend.md)
- [Data Bar](ui-data-bar.md)
- [Day Chart](ui-day-chart.md)
- [Line Chart](ui-line-chart.md)
- [Pie Chart](ui-pie-chart.md)

### Form

- [Checkbox](ui-checkbox.md)
- [Input Color](ui-input-color.md)
- [Input Date](ui-input-date.md)
- [Input Stepper](ui-input-stepper.md)
- [Input Text](ui-input-text.md)
- [Input Textarea](ui-input-textarea.md)
- [Label](ui-label.md)
- [Radio](ui-radio.md)
- [Range](ui-range.md)
- [Select](ui-select.md)
- [Toggle](ui-toggle.md)

### Menu

- [Menu Button](ui-menu-button.md)
- [Menu Group](ui-menu-group.md)
- [Menu Header](ui-menu-header.md)
- [Menu Link](ui-menu-link.md)
- [Menu Separator](ui-menu-separator.md)
- [Content Menu](ui-content-menu.md)
- [Header Menu](ui-header-menu.md)
- [Popup Menu](ui-popup-menu.md)
- [Side Menu](ui-side-menu.md)
