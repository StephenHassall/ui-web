# `<ui-code>`

Displays a code sample in a styled `<pre>` block, with optional regular-expression-based syntax coloring for HTML, CSS, JavaScript, SQL, and JSON. Without a language attribute, it displays plain text.

## Import

```js
import ".../ui-web/ui-code.js";
```

The component is registered as `<ui-code>` and renders into an open shadow root.

## Provide source code

Choose one source method:

1. Put the sample in an HTML comment directly inside `<ui-code>`. The component reads the first direct comment child, trims blank outer lines, and removes common leading indentation.
2. Set `src` to a URL. The component fetches that resource as text and displays it.

Embedded source:

```html
<ui-code javascript>
  <!--
  const answer = 42;
  console.log(answer);
  -->
</ui-code>
```

External source:

```html
<ui-code javascript src="examples/example.js"></ui-code>
```

The `src` URL is passed to `fetch()` and resolves relative to the document URL. The source attribute is the only observed attribute. The component does not check the fetch response's HTTP status before reading its text.

## Language attributes

Add one of these boolean attributes to select syntax coloring:

| Attribute | Formatting |
| --- | --- |
| `html` | HTML comments, tags, tag names, attributes, and quoted attribute values |
| `css` | CSS comments, declarations, selectors, functions, colors, variables, numbers, and constants |
| `javascript` | JavaScript comments, keywords, strings, numbers, function names, properties, and class names |
| `sql` | SQL comments, commands, identifiers, strings, numbers, constants, and operators |
| `json` | JSON properties, strings, numbers, constants, delimiters, and punctuation |
| none | Plain text |

If multiple language attributes are present, the selection order is `html`, `css`, `javascript`, `sql`, then `json`; the first matching attribute wins.

## Styling

Set CSS custom properties on `<ui-code>` or an ancestor. The code panel uses a `<pre>` element with horizontal overflow scrolling.

### General

| Variable | Default |
| --- | --- |
| `--ui-code-background` | `var(--ui-midground, hsl(0 0% 90% / 1))` |
| `--ui-code-foreground` | `var(--ui-default-foreground, black)` |
| `--ui-code-font-family` | System monospace font stack ending in `monospace` |
| `--ui-code-font-size` | `var(--ui-font-size-normal, 16px)` |

### Syntax colors

| Language | Variables |
| --- | --- |
| HTML | `--ui-code-html-comment`, `--ui-code-html-tag-bracket`, `--ui-code-html-tag-name`, `--ui-code-html-attribute`, `--ui-code-html-value` |
| CSS | `--ui-code-css-comment`, `--ui-code-css-class`, `--ui-code-css-id`, `--ui-code-css-tag`, `--ui-code-css-attribute`, `--ui-code-css-text`, `--ui-code-css-function`, `--ui-code-css-property`, `--ui-code-css-value`, `--ui-code-css-color`, `--ui-code-css-variable`, `--ui-code-css-constant` |
| JavaScript | `--ui-code-javascript-comment`, `--ui-code-javascript-keyword`, `--ui-code-javascript-declaration`, `--ui-code-javascript-property`, `--ui-code-javascript-class`, `--ui-code-javascript-function`, `--ui-code-javascript-text`, `--ui-code-javascript-numeric` |
| SQL | `--ui-code-sql-comment`, `--ui-code-sql-command`, `--ui-code-sql-text`, `--ui-code-sql-numeric`, `--ui-code-sql-identifier`, `--ui-code-sql-constant`, `--ui-code-sql-operator` |
| JSON | `--ui-code-json-property`, `--ui-code-json-text`, `--ui-code-json-literal`, `--ui-code-json-constant`, `--ui-code-json-colon`, `--ui-code-json-comma`, `--ui-code-json-curly-brackets`, `--ui-code-json-square-brackets` |

Example:

```html
<ui-code
  javascript
  style="
    --ui-code-background: #f5f5f5;
    --ui-code-foreground: #222;
    --ui-code-javascript-keyword: blue;
    --ui-code-javascript-numeric: darkgreen;">
  <!-- const answer = 42; -->
</ui-code>
```

## Formatting behavior and limitations

The highlighter uses regex replacements rather than language parsers. It is intended for readable examples, not complete or error-checked syntax parsing. Complex or unusual syntax may be only partly colored.

- JavaScript numeric highlighting recognizes common decimal and hexadecimal values. It includes array elements following `[` or `,`, such as `const values = [0, 3, 8];`. Many modern numeric forms and some operator contexts are not recognized.
- CSS and HTML formatting is heuristic; quoted delimiters, complex selectors, and declarations without semicolons may not be parsed as expected.
- Embedded comment source cannot contain an inner HTML comment without affecting how the browser parses the source container. Use `src` for examples that need literal HTML comments.
- Source mutations trigger reformatting for embedded-comment content. The component observes child-list changes, not all possible changes to text or language attributes.
- The source is inserted into the shadow `<pre>` as generated HTML. Use trusted code samples; this component is a display helper, not a sanitizer for untrusted source.

