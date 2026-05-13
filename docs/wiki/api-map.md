# API Map

This page is a quick index of the main modules, classes, and functions in PyFlo.

## Package Modules

| Module | Purpose |
| --- | --- |
| `pyflo.basins` | Base basin class shared by hydrology methods. |
| `pyflo.build` | Network traversal helpers for ordering links around a node. |
| `pyflo.constants` | Hydraulic and hydrologic constants. |
| `pyflo.distributions` | Equation evaluation and ordered-pair interpolation. |
| `pyflo.links` | Link, reach, weir, and hydraulic calculations. |
| `pyflo.networks` | Network and node containers. |
| `pyflo.routing` | Tailwater, reservoir, and storage routing analysis. |
| `pyflo.sections` | Hydraulic cross-section classes. |
| `pyflo.system` | CSV input and output helpers. |
| `pyflo.geometry.vertical` | Vertical profile and curve calculations. |
| `pyflo.nrcs.hydrology` | NRCS curve number and unit hydrograph hydrology. |
| `pyflo.rational.hydrology` | Rational Method basin hydrology. |
| `pyflo.rational.hydraulics` | Rational Method network hydraulic analysis. |

## Core Classes

| Class | Module | Notes |
| --- | --- | --- |
| `Basin` | `pyflo.basins` | Base area container. |
| `Evaluator` | `pyflo.distributions` | Evaluates equation strings and builds distributions. |
| `Link` | `pyflo.links` | Base upstream-to-downstream link. |
| `Reach` | `pyflo.links` | Conveyance link with Manning, critical, HGL, and loss methods. |
| `Weir` | `pyflo.links` | Outlet link for weir/orifice flow. |
| `Node` | `pyflo.networks` | Holds links, basin, and reservoir references. |
| `Network` | `pyflo.networks` | Holds nodes and exposes aggregate links, reaches, and basins. |
| `Tailwater` | `pyflo.routing` | Interpolated time-stage relationship. |
| `Reservoir` | `pyflo.routing` | Elevation-area-storage relationship. |
| `Analysis` | `pyflo.routing` | Time-step routing analysis. |
| `Section` | `pyflo.sections` | Base section interface. |
| `Circle` | `pyflo.sections` | Circular section. |
| `Rectangle` | `pyflo.sections` | Rectangular section. |
| `Square` | `pyflo.sections` | Square section. |
| `Trapezoid` | `pyflo.sections` | Trapezoidal section. |
| `Irregular` | `pyflo.sections` | Irregular ground-line section. |
| `Profile` | `pyflo.geometry.vertical` | Vertical alignment container. |
| `Point` | `pyflo.geometry.vertical` | Vertical profile point. |
| `Basin` | `pyflo.nrcs.hydrology` | NRCS curve number basin. |
| `Basin` | `pyflo.rational.hydrology` | Rational Method basin. |
| `Analysis` | `pyflo.rational.hydraulics` | Rational Method HGL analysis. |

## Important Functions

| Function | Module | Description |
| --- | --- | --- |
| `links_up_from_node(node, links)` | `pyflo.build` | Trace links upstream from a node. |
| `links_down_to_node(node, links)` | `pyflo.build` | Order links draining downstream to a node. |
| `links_down_from_node(node, links)` | `pyflo.build` | Trace links downstream from a node. |
| `links_up_to_node(node, links)` | `pyflo.build` | Order links upstream to a node. |
| `increment(array, interval)` | `pyflo.distributions` | Interpolate a two-column array to a regular interval. |
| `tuple_list_from_csv(filename)` | `pyflo.system` | Load CSV rows as float tuples. |
| `array_from_csv(filename)` | `pyflo.system` | Load CSV rows as a NumPy array. |
| `csv_from_tuple_list(filename, data)` | `pyflo.system` | Write float tuple rows to CSV. |
| `totaled_basin_data(node)` | `pyflo.rational.hydraulics` | Return cumulative basin data by reach. |

## Common Analysis Outputs

Rational HGL analysis returns an ordered dictionary keyed by reach. Values include `area`, `c`, `tc_local`, `tc_total`, `flow`, `hgl_1`, and `hgl_2`.

Routing analysis returns an ordered dictionary keyed by link. Values include a `data` list of rows containing `time`, `inflow`, `outflow`, `storage`, and `stage`.
