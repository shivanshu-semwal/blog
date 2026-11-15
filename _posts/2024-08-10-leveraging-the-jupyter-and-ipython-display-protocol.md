---
title: "Leveraging the Jupyter and IPython display protocol"
layout: post
tags:
  - python
  - jupyter-notebook
description: Using IPython rich display protocol methods like _repr_html_ and _repr_png_.
---

In Python, every object has a `__repr__` method used when the object is printed. In IPython, we can provide richer representations such as HTML, images, or LaTeX.

```python
class Student:
    def __init__(self, name, class_name) -> None:
        self.name = name
        self.class_name = class_name

    def __repr__(self):
        return f"Hi! I'm {self.name} of {self.class_name}"

s = Student("totoro", "5")
print(s)
```

IPython also supports special methods like:

- `_repr_html_()`
- `_repr_markdown_()`
- `_repr_latex_()`
- `_repr_png_()`
- `_repr_svg_()`

The default display is chosen according to a MIME priority order.

```python
class MultiMime:
    def __repr__(self):
        return "this is the repr"

    def _repr_html_(self):
        return "This <b>is</b> html"

    def _repr_markdown_(self):
        return "This **is** markdown"

    def _repr_latex_(self):
        return "$ Latex \otimes mimetype $"
```

The same idea can be used to create rich custom objects in notebooks. You can also register custom formatters for different object types.

```python
ipython = get_ipython()
html_formatter = ipython.display_formatter.formatters['text/html']
html_formatter.for_type(list, html_list_formatter)
```
