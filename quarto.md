# Quarto reference guide

Here are some of the markdown basics we can use with [Quarto](https://quarto.org/docs/authoring/markdown-basics.html).

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

![Elephant](elephant.png){width=80%}

![An Elephant](elephant.png){#fig-elephant}

This is illustrated well by @fig-elephant.

Here is a footnote reference,[^1] and another.[^longnote].

Here is an inline note.^[Inlines notes are easier to write, since you don't have to pick an identifier and move down to type the note.]

[^1]: Here is the footnote.

[^longnote]: Here's one with multiple blocks.

| Right | Left | Default | Center |
|------:|:-----|---------|:------:|
|   12  |  12  |    12   |    12  |
|  123  |  123 |   123   |   123  |
|    1  |    1 |     1   |     1  |

: My Caption {#tbl-letters}

See @tbl-letters.

Life is good [@Citation].

```python
1 + 1
```

inline math: $E = mc^{2}$

display math:

$$E = mc^{2}$$

```{mermaid}
flowchart LR
  A[Hard edge] --> B(Round edge)
  B --> C{Decision}
  C --> D[Result one]
  C --> E[Result two]
```

{{< video https://www.youtube.com/embed/wo9vZccmqwc >}}

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

---
title: "matplotlib demo"
format:
  html:
    code-fold: true
jupyter: python3
---

For a demonstration of a line plot on a polar axis, see @fig-polar.

```{python}
#| label: fig-polar
#| fig-cap: "A line plot on a polar axis"

import numpy as np
import matplotlib.pyplot as plt

r = np.arange(0, 2, 0.01)
theta = 2 * np.pi * r
fig, ax = plt.subplots(
  subplot_kw = {'projection': 'polar'} 
)
ax.plot(theta, r)
ax.set_rticks([0.5, 1, 1.5, 2])
ax.grid(True)
plt.show()
```
