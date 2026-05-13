# PyFlo Wiki

PyFlo is a Python library for stormwater hydrology and hydraulics. The package provides reusable building blocks for basin runoff calculations, open-channel and pipe hydraulics, network hydraulic grade line analysis, reservoir bleed-down simulations, and vertical profile calculations.

This wiki is organized around the way PyFlo models a drainage system:

1. Describe hydrology with basins and rainfall or intensity distributions.
2. Describe conveyance with sections, reaches, weirs, nodes, and networks.
3. Run an analysis that combines hydrology and hydraulics.
4. Inspect arrays, dictionaries, or plots produced by the model.

## Wiki Pages

- [Quick Start](quick-start.md): install PyFlo and build a minimal Rational Method network.
- [Core Concepts](core-concepts.md): learn how basins, nodes, links, sections, and distributions fit together.
- [Hydrology](hydrology.md): use Rational Method and NRCS unit hydrograph basin tools.
- [Hydraulics](hydraulics.md): compute section geometry, normal depth, HGL, and network results.
- [Routing](routing.md): run storage-based reservoir and outlet simulations.
- [Geometry](geometry.md): work with vertical profiles and irregular cross sections.
- [Data Resources](data-resources.md): understand the CSV and JSON resource files shipped with the repository.
- [API Map](api-map.md): find the main modules, classes, functions, and expected inputs.

## Units

PyFlo's calculations are written for customary drainage design units:

| Quantity | Typical unit |
| --- | --- |
| Time of concentration | minutes |
| Hydrograph time | hours |
| Rainfall depth | inches |
| Rainfall intensity | inches per hour |
| Basin area | acres |
| Elevation, depth, length | feet |
| Flow | cubic feet per second |
| Storage | cubic feet |

The library does not perform general unit conversion, so keep inputs consistent with the method being used.

## Recommended Reading Path

If you are new to the repository, start with [Quick Start](quick-start.md), then read [Core Concepts](core-concepts.md). After that, jump to the workflow page that matches your task: [Hydrology](hydrology.md), [Hydraulics](hydraulics.md), or [Routing](routing.md).
