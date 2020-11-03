## Intro
This project wraps a python interface around Duet Controllers running Reprap Firmware 3+.

It's intended to give you a clean python-driven motion control interface for building applications on top of machines using Duet Controllers.

## Supported Interfaces
* HTTP
* **Planned, but not yet supported:** Native Raspberry Pi using [Duet Software Framework](https://github.com/Duet3D/DuetSoftwareFramework)

## Installation
The easiest way to install is with pip. From this directory (the one with this README in it), install with:
```bash
pip3 install -e .
```
This enables you to both import the package at the system python level while also being able to edit the source files.

If you do not want to edit the source, simply invoke:
```bash
pip3 install .
```

## A Minimal Example
```python

from duetpy.duet import Duet

jubilee = Duet()

jubilee.home_all()

# These moves will be queued into the machine's buffer
jubilee.move_xyz_absolute(z=20)
jubilee.move_xyz_absolute(x=10, y=10)
jubilee.move_xyz_relative(x=10)
jubilee.move_xyz_relative(y=10)
jubilee.move_xyz_relative(x=10)
jubilee.move_xyz_relative(y=10)
jubilee.move_xyz_absolute(150, 150, wait=True)
# Asserting a wait ensures that the machine will finish moving before any additional code is executed.
```

## Warnings/Recommendations
### Multiple Connections
While it is possible to run multiple instances of this code from multiple sources (multiple computers, etc) in HTTP mode, only one source should perform machine 'writes' while all the others should be 'reads'.
This is such that applications built with this library can safely assume that it is the only thread of execution that is changing the Duet's state.

### Production Use Recommendations
If you are connecting to the Duet controller over HTTP, bear in mind that any network lag or hiccups between the computer running this python library and the machine may manifest as lag in the machine behavior.
