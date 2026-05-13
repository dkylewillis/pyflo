# Geometry

PyFlo includes geometry utilities for vertical alignments and irregular hydraulic sections.

## Vertical Profiles

`pyflo.geometry.vertical.Profile` stores profile points by station. Each point has a station, elevation, and optional vertical curve length.

```python
from pyflo.geometry.vertical import Profile

profile = Profile()
profile.create_pt(1000.0, 50.0)
profile.create_pt(1200.0, 54.0, length=100.0)
profile.create_pt(1500.0, 52.0)

elevation = profile.elevation(1225.0)
slope = profile.slope(1225.0)
stations = profile.key_stations(decimals=2)
```

Useful `Profile` methods:

| Method | Description |
| --- | --- |
| `create_pt(station, elevation, length=0.0)` | Add and sort a profile point. |
| `slope(station)` | Get profile slope at a station. |
| `elevation(station)` | Get profile elevation at a station. |
| `key_stations(decimals, curve_step=None, include=None, extremum=True, pvc=True, pvt=True)` | Return important station values. |

Each `Point` can report grades, PVC/PVT stations, PVC/PVT elevations, `k`, `r`, and extremum station.

## Irregular Sections

`sections.Irregular` accepts ground line points and computes wetted geometry for a supplied depth from the lowest point.

```python
from pyflo import sections

points = [
    (0.0, 0.0),
    (5.0, -2.1),
    (10.0, -3.4),
    (15.0, -5.6),
    (20.0, -4.7),
    (25.0, 0.0),
]

section = sections.Irregular(points, n=0.035)
area = section.flow_area(depth=5.6)
perimeter = section.wet_perimeter(depth=5.6)
```

Use irregular sections when a channel cannot be described as a simple circle, rectangle, square, or trapezoid.
