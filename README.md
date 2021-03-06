
[![Binder](http://mybinder.org/images/logo.svg)](http://mybinder.org/repo/hainm/nglview-notebooks)
[![DOI](https://zenodo.org/badge/11846/arose/nglview.svg)](https://zenodo.org/badge/latestdoi/11846/arose/nglview)
[![Build Status](https://travis-ci.org/arose/nglview.svg?branch=master)](https://travis-ci.org/arose/nglview)

![nglview](nglview.png)


An [IPython/Jupyter](http://jupyter.org/) widget to interactively view molecular structures and trajectories. Utilizes the embeddable [NGL Viewer](https://github.com/arose/ngl) for rendering. Support for showing data from the file-system, [RCSB PDB](http:www.rcsb.org), [simpletraj](https://github.com/arose/simpletraj) and from objects of analysis libraries [mdtraj](http://mdtraj.org/), [pytraj](http://amber-md.github.io/pytraj/latest/index.html), [mdanalysis](http://www.mdanalysis.org/).

Should work with Python 2 and 3. If you experience problems, please file an [issue](https://github.com/arose/nglview/issues).

Table of contents
=================

* [Installation](#installation)
* [Usage](#usage)
* [Interface classes](doc/interface_classes.md)
* [Changelog](CHANGELOG.md)
* [License](#license)


Installation
============

From PyPI:

    pip install nglview
Note: The above will try to install `jupyter`, `traitlets` and `ipywidgets`  as dependencies. If that fails install it manually `pip install jupyter`.

From Conda

    conda install -c omnia nglview


Usage
=====

Open a notebook

    jupyter notebook

and issue

```Python
import nglview
view = nglview.show_pdbid("3pqr")  # load "3pqr" from RCSB PDB and display viewer widget
view
```

A number of convenience functions are available to quickly display data from
the file-system, [RCSB PDB](http:www.rcsb.org), [simpletraj](https://github.com/arose/simpletraj) and from objects of analysis libraries [mdtraj](http://mdtraj.org/), [pytraj](http://amber-md.github.io/pytraj/latest/index.html), [mdanalysis](http://www.mdanalysis.org/), [ParmEd](http://parmed.github.io/ParmEd/).

| Function                                 | Description                                           |
|------------------------------------------|-------------------------------------------------------|
| `show_structure_file(path)`              | Shows structure (pdb, gro, mol2, sdf) in `path`       |
| `show_pdbid(pdbid)`                      | Shows `pdbid` fetched from RCSB PDB                   |
| `show_simpletraj(struc_path, traj_path)` | Shows structure & trajectory loaded with `simpletraj` |
| `show_mdtraj(traj)`                      | Shows `MDTraj` trajectory `traj`                      |
| `show_pytraj(traj)`                      | Shows `PyTraj` trajectory `traj`                      |
| `show_parmed(structure)`                 | Shows `ParmEd` structure
| `show_mdanalysis(univ)`                  | Shows `MDAnalysis` Universe or AtomGroup `univ`       |

API
===

### Representations

```python
view.add_cartoon("protein", color="residueindex")
view.add_surface("protein", opacity=0.3)
```

Representations can also be changed by overwriting the `representations` property
of the widget instance `view`. The available `type` and `params` are described
in the NGL Viewer [documentation](http://arose.github.io/ngl/doc).

```Python
view.representations = [
    {"type": "cartoon", "params": {
        "sele": "protein", "color": "residueindex"
    }},
    {"type": "ball+stick", "params": {
        "sele": "hetero"
    }}
]
```

The widget constructor also accepts a `representation` argument:

```Python
initial_repr = [
    {"type": "cartoon", "params": {
        "sele": "protein", "color": "sstruc"
    }}
]

view = nglview.NGLWidget(struc, representation=initial_repr)
view
```

### Properties

```Python
# set the frame number
view.frame = 100
```

```Python
# parameters for the NGL stage object
view.parameters = {
    # "percentages, "dist" is distance too camera in Angstrom
    "clipNear": 0, "clipFar": 100, "clipDist": 10,
    # percentages, start of fog and where on full effect
    "fogNear": 0, "fogFar": 100,
    # background color
    "theme": "dark",
}
```

### Multiple widgets

You can have multiple widgets per notebook cell:

```Python
from ipywidgets.widgets import Box
w1 = NGLWidget(...)
w2 = NGLWidget(...)
Box(children=(w1,w2))
```


License
=======

Generally MIT, see the LICENSE file for details.
