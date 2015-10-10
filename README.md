parfait
=======

parfait is a wrapper around common pyqgis idioms that allows for faster development and hopefully removes
some of the (sometimes) ugly C/C++ based API (read: not Python-ish)

The current plan is to flesh out ideas using parfait and then mirgate them to core QGIS as they become stable or well designed.

What can it do so far?
----------------------

**Note: If you are using parfait in a standalone QGIS Python app you need to call `QGIS.init()` before doing anything else.

### Opening a project

```
from parfait import QGIS, open_project

canvas = QgsMapCanvas()
with open_project(pfile, canvas=canvas) as project:
    print project
```

or without the `with` block

```
from parfait import QGIS, open_project

app = QGIS.init()
canvas = QgsMapCanvas()
project = open_project(pfile, canvas=canvas)
print project
```

### Rendering a composer template from a file.

```python
import sys
from qgis.gui import QgsMapCanvas
from parfait import QGIS, render_template, map_layers, open_project

pfile = r"project.qgs"
template = r"template.qpt"

app = QGIS.init()
canvas = QgsMapCanvas()
with open_project(pfile, canvas=canvas) as project:
    settings = project.map_settings
    render_template(template, settings, canvas, r"out.pdf")
```

or without the `with` block

```python
canvas = QgsMapCanvas()
project = open_project(pfile, canvas=canvas)
settings = project.map_settings
render_template(template, settings, canvas, r"out.pdf")
project.close()
```

### Listing loaded layers

```python
from wrappers import map_layers
layers = map_layers()
mylayer = map_layers(name='mylayer')
```

filter by regex

```
from wrappers import map_layers
for layer in map_layers(".*Bound.*"):
    print layer.name()
```


