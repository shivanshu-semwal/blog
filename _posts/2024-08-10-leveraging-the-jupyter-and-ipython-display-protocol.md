---
title: "Leveraging the Jupyter and IPython display protocol"
layout: post
tags:
  - python
  - jupyter-notebook
description: Using IPython rich display protocol methods like _repr_html_ and _repr_png_.
---


In python for every object you create there is a `__repr__` method in it which is used to display it wherever you print that variable.

```python
class Student:

    def __init__(self, name, class_name) -> None:
        self.name = name
        self.class_name = class_name

    def __repr__(self):
        return f"Hi! I'm {self.name} of {self.class_name}"

if __name__ == "__main__":
    s = Student("totoro", "5")
    print(s) # Output: Hi! I'm totoro of 5
```

Same in true for Ipython also, but in this case we can give other type of representations like HTML, images, latex.

Example:

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

If you run this in jupyter notebook, you will get the `_repr_html_` display.

There is a priority on which type of mimetype will be used,

```yml
sphinx:
  config:
    nb_mime_priority_overrides: [
      ['html', 'application/vnd.jupyter.widget-view+json', 10],
      ['html', 'application/javascript', 20],
      ['html', 'text/html', 30],
      ['html', 'image/svg+xml', 40],
      ['html', 'image/png', 50],
      ['html', 'image/gif', 60],
      ['html', 'image/jpeg', 70],
      ['html', 'text/markdown', 80],
      ['html', 'text/latex', 90],
      ['html', 'text/plain', 100]
    ]
```

More details can be found [here](https://jupyterbook.org/en/stable/content/code-outputs.html).

Here is an example of a `Gaussian` class which has representations in multiple formats.

```python
%matplotlib inline
import numpy as np
import matplotlib.pyplot as plt

from IPython.core.pylabtools import print_figure
from IPython.display import Image, SVG, Math

class Gaussian(object):
    """A simple object holding data sampled from a Gaussian distribution.
    """
    def __init__(self, mean=0, std=1, size=1000):
        self.data = np.random.normal(mean, std, size)
        self.mean = mean
        self.std = std
        self.size = size
        # For caching plots that may be expensive to compute
        self._png_data = None
        self._svg_data = None
        
    def _figure_data(self, format):
        fig, ax = plt.subplots()
        ax.plot(self.data, 'o')
        ax.set_title(self._repr_latex_())
        data = print_figure(fig, format)
        # We MUST close the figure, otherwise IPython's display machinery
        # will pick it up and send it as output, resulting in a double display
        plt.close(fig)
        return data
    
    # Here we define the special repr methods that provide the IPython display protocol
    # Note that for the two figures, we cache the figure data once computed.
    
    def _repr_png_(self):
        if self._png_data is None:
            self._png_data = self._figure_data('png')
        return self._png_data


    def _repr_svg_(self):
        if self._svg_data is None:
            self._svg_data = self._figure_data('svg')
        return self._svg_data
    
    def _repr_latex_(self):
        return r'$\mathcal{N}(\mu=%.2g, \sigma=%.2g),\ N=%d$' % (self.mean, self.std, self.size)
    
    # We expose as properties some of the above reprs, so that the user can see them
    # directly (since otherwise the client dictates which one it shows by default)
    @property
    def png(self):
        return Image(self._repr_png_(), embed=True)
    
    @property
    def svg(self):
        return SVG(self._repr_svg_())
        
    @property
    def latex(self):
        return Math(self._repr_latex_())
    
    # An example of using a property to display rich information, in this case
    # the histogram of the distribution.  We've hardcoded the format to be png
    # in this case, but in production code it would be trivial to make it an option
    @property
    def hist(self):
        fig, ax = plt.subplots()
        ax.hist(self.data, bins=100)
        ax.set_title(self._repr_latex_())
        data = print_figure(fig, 'png')
        plt.close(fig)
        return Image(data, embed=True)
```

By default if you display a object of this class, it will display the represenation with highest priority.
For other representations you can use the property directly

```python
x = Gaussian()
x # shows default one
x.svg # shows svg
x.png # shows png

# a additional usecase
x.hist # shows histogram
```

You can also register custom formatters for different types of objects. This allows creation of third party libraries like disp, although it has not been updated for a long time. [Link for disp repo](https://github.com/ipython/disp)

Here is how you register custom formatter:

```py
ipython = get_ipython()
html_formatter = ipython.display_formatter.formatters['text/html']

# registering for type list and formatter html_list_formatter
html_formatter.for_type(list, html_list_formatter) 
```
