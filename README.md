# Robotics, Vision & Control: 3rd edition in Python

<img src="https://github.com/petercorke/RVC3-python/raw/main/doc/frontcover.png" alt="Front cover 978-3-031-06468-5_5208" width="300">

This book depends on the following open-source Python packages:

- [Robotics Toolbox for Python](https://github.com/petercorke/robotics-toolbox-python)
- [Machine Vision Toolbox for Python](https://github.com/petercorke/machinevision-toolbox-python)

which in turn have dependencies on other packages created by the author and third parties.

## Installing the package

This package provides a simple one-step installation of the required Toolboxes
```
$ pip install rvc3python
```
or
```
$ conda install rvc3python
```

### Installing into a Conda environment

It's probably a good idea to create a virtual environment to keep this package and its dependencies
separated from your other Python code and projects.  This is really easy using Conda 
conda, and only adds a couple of extra lines
```
$ conda create -n RVC3 python=3.10
$ conda activate RVC3
$ pip install rvc3python
```

### Installing deep learning tools

Chapter 11 has some deep learning examples based on PyTorch.  If you don't have 
PyTorch installed you can use an install option
```
$ pip install rvc3python[pytorch]
```
or
```
$ conda install rvc3python[pytorch]
```
## Using the Toolboxes

The simplest way to get going is to use the command line tool

```
$ rvctool
 ____       _           _   _             __     ___     _                ___      ____            _             _   _____ 
|  _ \ ___ | |__   ___ | |_(_) ___ ___    \ \   / (_)___(_) ___  _ __    ( _ )    / ___|___  _ __ | |_ _ __ ___ | | |___ / 
| |_) / _ \| '_ \ / _ \| __| |/ __/ __|    \ \ / /| / __| |/ _ \| '_ \   / _ \/\ | |   / _ \| '_ \| __| '__/ _ \| |   |_ \ 
|  _ < (_) | |_) | (_) | |_| | (__\__ \_    \ V / | \__ \ | (_) | | | | | (_>  < | |__| (_) | | | | |_| | | (_) | |  ___) |
|_| \_\___/|_.__/ \___/ \__|_|\___|___( )    \_/  |_|___/_|\___/|_| |_|  \___/\/  \____\___/|_| |_|\__|_|  \___/|_| |____/ 
                                      |/                                                                                   
for Python (RTB==1.0.2, MVTB==0.9.1, SMTB==1.0.0)

import numpy as np
import scipy as sp
import matplotlib.pyplot as plt
from math import pi
from spatialmath import *
from spatialmath.base import *
from roboticstoolbox import *
from machinevisiontoolbox import *
import machinevisiontoolbox.base as mvbase

func/object?       - show brief help
help(func/object)  - show detailed help
func/object??      - show source code


Results of assignments will be displayed, use trailing ; to suppress

 
Python 3.8.5 (default, Sep  4 2020, 02:22:02) 
Type 'copyright', 'credits' or 'license' for more information
IPython 8.0.1 -- An enhanced Interactive Python. Type '?' for help.


>>> 
```

This provides an interactive Python (IPython) session with all the Toolboxes preloaded, and ready
to go.  It's a convenient and "MATLAB-like"
workbench environment for robotics and computer vision.

For example to load an ETS model of a Panda robot, solve a forward kinematics and
inverse kinematics problem, and an interactive graphical display is simply:

```
>>> panda = models.ETS.Panda()
ERobot: Panda (by Franka Emika), 7 joints (RRRRRRR)
┌─────┬───────┬───────┬────────┬─────────────────────────────────────────────┐
│link │ link  │ joint │ parent │             ETS: parent to link             │
├─────┼───────┼───────┼────────┼─────────────────────────────────────────────┤
│   0 │ link0 │     0 │ BASE   │ tz(0.333) ⊕ Rz(q0)                          │
│   1 │ link1 │     1 │ link0  │ Rx(-90°) ⊕ Rz(q1)                           │
│   2 │ link2 │     2 │ link1  │ Rx(90°) ⊕ tz(0.316) ⊕ Rz(q2)                │
│   3 │ link3 │     3 │ link2  │ tx(0.0825) ⊕ Rx(90°) ⊕ Rz(q3)               │
│   4 │ link4 │     4 │ link3  │ tx(-0.0825) ⊕ Rx(-90°) ⊕ tz(0.384) ⊕ Rz(q4) │
│   5 │ link5 │     5 │ link4  │ Rx(90°) ⊕ Rz(q5)                            │
│   6 │ link6 │     6 │ link5  │ tx(0.088) ⊕ Rx(90°) ⊕ tz(0.107) ⊕ Rz(q6)    │
│   7 │ @ee   │       │ link6  │ tz(0.103) ⊕ Rz(-45°)                        │
└─────┴───────┴───────┴────────┴─────────────────────────────────────────────┘

┌─────┬─────┬────────┬─────┬───────┬─────┬───────┬──────┐
│name │ q0  │ q1     │ q2  │ q3    │ q4  │ q5    │ q6   │
├─────┼─────┼────────┼─────┼───────┼─────┼───────┼──────┤
│  qr │  0° │ -17.2° │  0° │ -126° │  0° │  115° │  45° │
│  qz │  0° │  0°    │  0° │  0°   │  0° │  0°   │  0°  │
└─────┴─────┴────────┴─────┴───────┴─────┴───────┴──────┘

>>> panda.fkine(panda.qz)
   0.7071    0.7071    0         0.088     
   0.7071   -0.7071    0         0         
   0         0        -1         0.823     
   0         0         0         1      
>>> panda.ikine_LM(SE3.Trans(0.4, 0.5, 0.2) * SE3.Ry(pi/2))
IKSolution(q=array([  -1.849,   -2.576,   -2.914,     1.22,   -1.587,    2.056,   -1.013]), success=True, iterations=13, searches=1, residual=3.3549072615799585e-10, reason='Success')
>>> panda.teach(panda.qz)
```
![](https://github.com/petercorke/RVC3-python/raw/main/doc/panda_noodle.png)

Computer vision is just as easy.  For example, we can import an image, blur it and display it alongside the
original
```
>>> mona = Image.Read("monalisa.png")
>>> Image.Hstack([mona, mona.smooth(sigma=5)]).disp()
```
![](https://github.com/petercorke/machinevision-toolbox-python/raw/master/figs/mona%2Bsmooth.png)

or load two images of the same scene, compute SIFT features and display putative matches
```
>>> sf1 = Image.Read("eiffel-1.png", mono=True).SIFT()
>>> sf2 = Image.Read("eiffel-2.png", mono=True).SIFT()
>>> matches = sf1.match(sf2)
>>> matches.subset(100).plot("w")
```
![](https://github.com/petercorke/machinevision-toolbox-python/raw/master/figs/matching.png)

`rvctool` is a wrapper around [IPython](https://ipython.readthedocs.io/en/stable) where:
- functions and classes can be accessed without needing package prefixes
- results are displayed by default like MATLAB does, and like MATLAB you need to put a semicolon on the
  end of the line to prevent this
- the prompt is the standard Python REPL prompt `>>>` rather than the IPython prompt, this can
  be overridden by a command-line switch
- allows cutting and pasting in lines from the book, and prompt characters are ignored

The Robotics, Vision & Control book uses `rvctool` for all the included examples.

`rvctool` imports the all the above mentioned packages using `import *` which is not considered
best Python practice.  It is very convenient for interactive experimentation, but in your own
code you can control the imports as you see fit.

## Other command line tools

This package provides additional command line tools including:
- `eigdemo`, animation showing linear transformation of rotating unit vector
- `tripleangledemo`, experiment with various triple-angle sequences
- `twistdemo`, experiment with 3D twists
# Block diagram models

<img src="https://github.com/petercorke/bdsim/raw/master/figs/BDSimLogo_NoBackgnd%402x.png" alt="bdsim logo" width="300">

Block diagram models are key to the pedagogy of the RVC3 book and 25 models are included.
To simulate these models we use the Python package [bdsim](https://github.com/petercorke/bdsim) which can run models:

- written in Python using [bdsim](https://github.com/petercorke/bdsim#getting-started) blocks and wiring.
- created graphically using [bdedit](https://github.com/petercorke/bdsim#bdedit-the-graphical-editing-tool) and saved as a `.bd` (JSON format) file.

The models are included in the `RVC3` package when it is installed and `rvctool` adds them
to the module search path.  This means you can invoke them from `rvctool` by
```
>>> %run -m vloop_test
```

If you want to directly access the folder containing the models
use the command line tool
```
$ bdsim_path
... opt/miniconda3/envs/RVC3b/lib/python3.8/site-packages/RVC3/models
```
to find where it is in the Python package install tree.




# Additional book resources

This GitHub repo provides additional resources for readers including:
- Jupyter notebooks containing all code lines from each chapter, see folder [`notebooks`](RVC3/notebooks/README.md)

- The code to produce every Python-generated figure in the book, see folder [`figures`](RVC3/figures/README.md)
- All example scripts, see folder [`examples`](RVC3/examples/README.md)

To get this material you need to clone the repo
```
$ git clone https://github.com/petercorke/RVC3-python.git
```