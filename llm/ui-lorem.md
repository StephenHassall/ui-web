# `<ui-lorem>`

`<ui-lorem>` generates random placeholder text from a built-in list of lorem-ipsum-style words. It creates one or more paragraph elements inside its shadow DOM.

## Import

```js
import ".../ui-web/ui-lorem.js";
```

## Basic usage

```html
<ui-lorem></ui-lorem>
```

By default, the component generates one paragraph containing a random 4–6 sentences, with 8–12 words per sentence.

## Attributes

| Attribute | Default | Effect |
| --- | ---: | --- |
| `paragraphs` | `1` | Number of paragraph elements to generate. |
| `sentences` | `5` | Base number of sentences in each paragraph. |
| `words` | `10` | Base number of words in each sentence. |
| `spread` | `20` | Percentage of random variation applied to both sentence and word counts. |

For example, `sentences="2" spread="0"` generates exactly two sentences per paragraph; `words="5" spread="0"` generates exactly five words per sentence.

```html
<ui-lorem paragraphs="2" sentences="3" words="8" spread="0"></ui-lorem>
```

Counts are chosen randomly and inclusively from the base count plus/minus the rounded spread amount. Minimum sentence and word counts are clamped to at least one. Attribute values are parsed as integers for `paragraphs`, `sentences`, and `words`, and as a floating-point number for `spread`.

The component regenerates its content when one of these attributes changes while it is connected. Generated paragraphs are rendered as `<p>` elements in the component's shadow DOM.

## Styling

The component has no component-specific CSS variables. Its stylesheet removes the top margin from the first generated paragraph and the bottom margin from the last. Style the paragraphs inside its shadow DOM using inherited styles or the component's surrounding layout where applicable.

