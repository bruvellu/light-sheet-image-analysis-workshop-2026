# Quarto reference guide

Here are some of the markdown basics we can use with [Quarto](https://quarto.org/docs/authoring/markdown-basics.html).

## Text formatting

*italics*, **bold**, ***bold italics***, superscript^2^ / subscript~2~, ~~strikethrough~~, `verbatim code`

* unordered list
  + sub-item 1
  + sub-item 2
    - sub-sub-item 1

- [ ] Task 1
- [x] Task 2

term
: definition

> Blockquote

## Figures

![Elephant](https://quarto.org/docs/authoring/elephant.png){width=80%}

![An Elephant](https://quarto.org/docs/authoring/elephant.png){#fig-elephant}

This is illustrated well by @fig-elephant.

## Tables

| Right | Left | Default | Center |
|------:|:-----|---------|:------:|
|   12  |  12  |    12   |    12  |
|  123  |  123 |   123   |   123  |
|    1  |    1 |     1   |     1  |

: My Caption {#tbl-letters}

See @tbl-letters.

## Citations

Life is good [@Citation]. (put bibref formatted in references.bib)

## Footnotes

Here is a footnote reference,[^1] and another.[^longnote].

Here is an inline note.^[Inlines notes are easier to write, since you don't have to pick an identifier and move down to type the note.]

[^1]: Here is the footnote.

[^longnote]: Here's one with multiple blocks.

## Code blocks and math

```python
1 + 1
```

inline math: $E = mc^{2}$

display math:

$$E = mc^{2}$$

## Videos

{{< video https://www.youtube.com/embed/wo9vZccmqwc >}}

## Callout blocks

:::{.callout-note}
Note callout block.
:::

:::{.callout-tip}
Tip callout block.
:::

:::{.callout-warning}
Warning callout block.
:::

:::{.callout-caution}
Warning callout block.
:::

:::{.callout-important}
Warning callout block.
:::

