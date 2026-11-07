# `<ui-theme>`

`<ui-theme>` applies a theme stylesheet to its own contents. Use it to display different themes side by side or to preview a theme without changing the page-wide theme.

## Import

```js
import ".../ui-web/ui-theme.js";
```

The module registers the custom element. The implementation is contained in `src/theme/ui-theme.js`; there are no separate component HTML or CSS source files. Its small base style is embedded in the JavaScript.

## Basic usage

```html
<ui-theme src="/node_modules/@coderundebug/ui-web/theme/light.css">
  <p>This content is shown in the light theme.</p>
  <ui-button>Button</ui-button>
</ui-theme>

<ui-theme src="/node_modules/@coderundebug/ui-web/theme/dark.css">
  <p>This content is shown in the dark theme.</p>
  <ui-button>Button</ui-button>
</ui-theme>
```

The built-in theme stylesheets are:

- `/node_modules/@coderundebug/ui-web/theme/light.css`
- `/node_modules/@coderundebug/ui-web/theme/dark.css`
- `/node_modules/@coderundebug/ui-web/theme/system.css`
- `/node_modules/@coderundebug/ui-web/theme/serif.css`
- `/node_modules/@coderundebug/ui-web/theme/mono.css`

Adjust the URL to match where the package's files are served in your application.

## Attribute

| Attribute | Behavior |
|---|---|
| `src` | URL of the theme CSS file to fetch and apply inside this `<ui-theme>` element. Changing it loads and applies the new stylesheet. |

`src` is the only observed attribute. The theme stylesheet is cached by URL for subsequent `<ui-theme>` instances that use the same source. The host has an open shadow root with a slot for its content. Its base styling uses the shared `--ui-font-size-normal`, `--ui-foreground`, and `--ui-background` variables.

Theme CSS variables set on the `<ui-theme>` host are inherited by its slotted content, including nested `ui-web` components. This scopes the theme preview to the component; it does not change the styles of the rest of the page.

## Page-wide theme switching

The module also exports the `UiTheme` class as its default export. Its static `switchPageTheme(source)` method is for changing a page-wide theme, not for changing one `<ui-theme>` instance:

```js
import UiTheme from ".../ui-web/ui-theme.js";

UiTheme.switchPageTheme("/node_modules/@coderundebug/ui-web/theme/dark.css");
```

Passing a source URL saves it in `localStorage` under `ui-theme`, adds a theme stylesheet link to `<head>`, and removes the previous `link[ui-theme]` after the new link loads. Call the method without a source to clear the saved selection and remove the current theme link:

```js
UiTheme.switchPageTheme();
```

To initialize page-wide dynamic themes, include the loader module with an optional default theme URL:

```html
<script
  type="module"
  src="/node_modules/@coderundebug/ui-web/ui-theme-load.js"
  blocking="render"
  async
  default-ui-theme="/node_modules/@coderundebug/ui-web/theme/system.css">
</script>
```

The loader uses the saved `ui-theme` URL from local storage when present; otherwise it uses the `default-ui-theme` attribute. It adds the selected CSS as a page-level stylesheet. If neither source is available, no theme link is added. `ui-theme-load.js` is the built package output for `src/theme/load.js`; it is separate from the `ui-theme.js` component module.

## Custom themes

`src/theme/*.css` contains the built-in themes. A custom theme is a CSS file served by your application; define tokens on both `:root` and `:host` so they work as a page-level theme and as a `<ui-theme>` preview:

```css
:root,
:host {
  --ui-background: gold;
  --ui-foreground: brown;
}
```

For page-wide styling of ordinary HTML (such as applying the theme colors and font to `body`), add those rules to the custom stylesheet as well. `<ui-theme>` itself provides the themed background, foreground, and normal font size for its host. See [default-css.md](default-css.md) for shared theme tokens and defaults.

## Loading behavior and caveats

- Each `src` URL is fetched and converted to a constructed `CSSStyleSheet`; the component needs browser support for constructed stylesheets and `adoptedStyleSheets`.
- The source does not check the fetch response status or handle fetch/stylesheet errors. Ensure the stylesheet URL exists and is served with accessible CSS content.
- Page-wide `switchPageTheme()` removes the previous theme link only after the new link's `load` event. If the new link fails to load, the previous link is not removed by this implementation.
- Page-wide switching accesses `localStorage`; this API does not handle storage access errors.
- Do not use page-wide switching when only one region should be themed. Use a `<ui-theme src="...">` wrapper for a local preview.
