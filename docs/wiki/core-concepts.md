# Core Concepts

PyFlo combines hydrology objects with hydraulic network objects. The same network container can hold basins, reservoirs, reaches, and outlet links.

## Basins

Basins represent drainage areas. The base `pyflo.basins.Basin` stores area and defines the expected `flood_data` and `flood_hydrograph` interface. Method-specific basin classes add runoff behavior:

| Class | Use |
| --- | --- |
| `pyflo.rational.hydrology.Basin` | Rational Method runoff from area, runoff coefficient, and time of concentration. |
| `pyflo.nrcs.hydrology.Basin` | NRCS curve number runoff, unit hydrographs, and flood hydrographs. |

## Sections

Sections describe cross-sectional geometry. They provide methods such as `flow_area(depth)`, `wet_perimeter(depth)`, `hyd_radius(depth)`, `surface_width(depth)`, and `projection(depth)`.

Available section classes include `Circle`, `Rectangle`, `Square`, `Trapezoid`, and `Irregular`.

## Nodes And Links

A `Network` contains `Node` objects. A node can hold one basin, one reservoir, and one or more links. Links connect an upstream node to a downstream node.

| Object | Role |
| --- | --- |
| `Network` | Top-level container for nodes, links, reaches, and basins. |
| `Node` | A connection point where basins, reservoirs, reaches, and weirs are assigned. |
| `Reach` | A pipe, culvert, ditch, or open channel with length, slope or inverts, section, and losses. |
| `Weir` | An outlet link that can compute weir or orifice flow depending on stage and section rise. |

Create links through node helper methods:

```python
network = networks.Network()
upstream = network.create_node()
downstream = network.create_node()

section = sections.Circle(diameter=2.0, n=0.013)
reach = upstream.create_reach(
    node_2=downstream,
    inverts=(10.0, 9.5),
    length=250.0,
    section=section,
)
```

## Distributions

Distributions are two-column ordered-pair arrays. The first column is usually time, and the second column is a depth, ratio, stage, or flow.

`pyflo.distributions.increment(array, interval)` interpolates an array to a regular interval. `pyflo.distributions.Evaluator` evaluates equation strings over a range, which is useful for intensity-duration-frequency curves.

## Traversal Direction

Analysis functions use helper functions in `pyflo.build` to order links upstream or downstream from a target node. For most workflows, pass the downstream node or outfall to the analysis class and let PyFlo trace the contributing network.
