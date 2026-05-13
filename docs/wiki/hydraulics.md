# Hydraulics

PyFlo's hydraulic tools are centered on sections, reaches, weirs, and network analysis.

## Section Geometry

Every section supports the same geometric interface:

```python
from pyflo import sections

pipe = sections.Circle(diameter=2.0, n=0.013)
depth = 1.0

area = pipe.flow_area(depth)
perimeter = pipe.wet_perimeter(depth)
radius = pipe.hyd_radius(depth)
width = pipe.surface_width(depth)
```

Common section constructors:

| Section | Constructor |
| --- | --- |
| Circle | `sections.Circle(diameter, count=1, n=None)` |
| Rectangle | `sections.Rectangle(span, rise, count=1, n=None)` |
| Square | `sections.Square(side, count=1, n=None)` |
| Trapezoid | `sections.Trapezoid(l_slope, b_width, r_slope, count=1, n=None)` |
| Irregular | `sections.Irregular(points, count=1, n=None)` |

For Manning calculations, set `n` on the section.

## Reach Hydraulics

`pyflo.links.Reach` represents a conveyance link between two nodes. It can be created directly or through `Node.create_reach`.

```python
from pyflo import links, sections

section = sections.Rectangle(span=2.0, rise=9999.0, n=0.012)
reach = links.Reach(section=section, slope=0.01)

flow = 3.0
depth = reach.normal_depth(flow)
velocity = reach.velocity(depth)
```

Useful reach methods:

| Method | Description |
| --- | --- |
| `normal_flow(depth)` | Manning flow at a supplied depth. |
| `normal_depth(flow)` | Solves depth for a supplied flow. |
| `critical_depth(flow)` | Solves critical depth. |
| `critical_velocity(flow)` | Velocity at critical depth. |
| `critical_slope(flow)` | Slope associated with critical flow. |
| `friction_slope(depth, flow)` | Water surface slope from Manning friction. |
| `friction_loss(depth, flow)` | Friction head loss over reach length. |
| `minor_loss(depth, flow)` | Minor loss from `k_minor`, if supplied. |
| `section_time(depth, flow)` | Travel time through the reach in minutes. |
| `hgl_1(stage_2, flow)` | Upstream hydraulic grade elevation. |
| `hgl_2(stage_2, flow)` | Downstream controlling hydraulic grade elevation. |

## Weirs And Orifices

`pyflo.links.Weir` uses the section shape and upstream and downstream stages to compute flow. If the section has a `rise`, the link behaves as an orifice once the upstream stage exceeds the top of the opening. If the section has no rise, it behaves as a weir.

```python
opening = sections.Circle(diameter=3.25 / 12.0)
weir = upstream.create_weir(
    node_2=outfall,
    invert=23.5,
    k_orif=0.6,
    k_weir=3.2,
    section=opening,
)

flow = weir.flow(stage_1=25.35, stage_2=0.0)
```

## Rational Method Network Analysis

`pyflo.rational.hydraulics.Analysis` traces reaches draining to a downstream node, totals contributing basin data, computes flow, adds travel time, and solves HGL elevations.

```python
from pyflo.rational import hydraulics

analysis = hydraulics.Analysis(
    node=outfall,
    tw=4.85,
    intensity=6.5,
)
data = analysis.hgl_solution_data()
```

Each result entry is keyed by a reach and contains:

| Key | Description |
| --- | --- |
| `area` | Cumulative contributing area. |
| `c` | Area-weighted runoff coefficient. |
| `tc_local` | Local or inherited time of concentration. |
| `tc_total` | Time of concentration plus reach travel time. |
| `flow` | Rational Method design flow. |
| `hgl_1` | Upstream HGL. |
| `hgl_2` | Downstream HGL. |

Use `pyflo.rational.hydraulics.totaled_basin_data(node)` when you only need cumulative basin area and runoff coefficient by reach.
