# Quick Start

This page builds a small Rational Method network and solves hydraulic grade line data through one reach.

## Install

Install from PyPI:

```bash
pip install pyflo
```

For local development from this repository:

```bash
pip install -r requirements/development.txt
python tests.py
```

## Build A One-Reach Network

```python
import json

from pyflo import distributions, networks, sections
from pyflo.rational import hydraulics, hydrology


network = networks.Network()
inlet = network.create_node()
outfall = network.create_node()

pipe = sections.Circle(diameter=1.5, n=0.012)
inlet.create_reach(
    node_2=outfall,
    inverts=(4.1, 4.0),
    length=65.76,
    section=pipe,
)

basin = hydrology.Basin(tc=10.0, area=1.58, c=0.276)
inlet.add_basin(basin)

with open("./resources/intensity/fdot.json") as data_file:
    fdot = json.load(data_file)

zone = next(item for item in fdot["zones"] if item["name"] == "FDOT ZONE 10")
frequency = next(item for item in zone["frequencies"] if item["duration"] == 3)

intensity = distributions.Evaluator(
    equation=fdot["equation"],
    x_key=fdot["input_key"],
    eq_kwargs=frequency["coefficients"],
    x_multi=60.0,
)

analysis = hydraulics.Analysis(
    node=outfall,
    tw=4.85,
    intensity=intensity,
)
results = analysis.hgl_solution_data()
```

`results` is an ordered dictionary keyed by `Reach` objects. Each value contains basin totals, flow, travel time, and upstream and downstream hydraulic grade elevations.

```python
for reach, data in results.items():
    print(round(data["flow"], 1), round(data["hgl_1"], 1), round(data["hgl_2"], 1))
```

## Next Steps

- Use [Hydrology](hydrology.md) to generate unit and flood hydrographs.
- Use [Hydraulics](hydraulics.md) to size sections and analyze reaches.
- Use [Routing](routing.md) to model reservoirs and outlet structures.
