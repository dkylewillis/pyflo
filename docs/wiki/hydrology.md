# Hydrology

PyFlo includes Rational Method and NRCS curve number hydrology tools. Both basin classes inherit from the base `pyflo.basins.Basin` and expose `flood_data` and `flood_hydrograph` style outputs.

## Rational Method Basins

Use `pyflo.rational.hydrology.Basin` for small drainage areas where Rational Method assumptions are appropriate.

```python
from pyflo.rational import hydrology

basin = hydrology.Basin(tc=10.0, area=0.75, c=0.85)
print(basin.runoff_area)  # area * c
```

You can also build a composite basin from `(area, c)` pairs:

```python
basin = hydrology.Basin(
    tc=10.0,
    shapes=((0.30, 0.95), (0.05, 0.20)),
)
```

For a rainfall distribution, `flood_hydrograph(rain_dist, interval)` returns a NumPy array of time-flow pairs. The Rational Method implementation uses rainfall depth divided by elapsed time as intensity at each interpolated point.

## NRCS Curve Number Basins

Use `pyflo.nrcs.hydrology.Basin` for curve number runoff and unit hydrograph workflows.

```python
from pyflo import system
from pyflo.nrcs import hydrology

runoff_dist = system.array_from_csv("./resources/distributions/runoff/scs484.csv")
basin = hydrology.Basin(
    area=4.6,
    cn=85.0,
    tc=2.3,
    runoff_dist=runoff_dist,
    peak_factor=484.0,
)
```

Important properties and methods:

| Member | Description |
| --- | --- |
| `potential_retention` | Curve number storage term, `1000 / cn - 10`. |
| `initial_abstraction` | Initial abstraction, `0.2 * potential_retention`. |
| `runoff_depth(rain_depth)` | Runoff depth for a cumulative rainfall depth. |
| `runoff_volume(rain_depth)` | Runoff volume in cubic feet. |
| `unit_hydrograph(interval)` | Unit hydrograph scaled by peak time and peak runoff. |
| `flood_hydrograph(rain_depths, interval)` | Composite runoff hydrograph from rainfall depths. |

## Rainfall And Runoff Arrays

Rainfall and runoff distributions are usually NumPy arrays with two columns:

```python
import numpy

rainfall_ratio = numpy.array([
    (0.00, 0.000),
    (0.50, 0.540),
    (1.00, 1.000),
])
rainfall_depths = rainfall_ratio * [6.0, 5.0]
```

This scales the first column to 6 hours and the second column to 5 inches.

## Intensity Equations

`pyflo.distributions.Evaluator` safely evaluates equation strings with math functions and named variables.

```python
from pyflo import distributions

intensity = distributions.Evaluator(
    equation="[a] + [b]*log([t]) + [c]*log([t])**2 + [d]*log([t])**3",
    x_key="[t]",
    eq_kwargs={"[a]": 11.32916, "[b]": -1.38557, "[c]": -0.36672, "[d]": 0.05012},
    x_multi=60.0,
)

print(intensity.get_y(10.0 / 60.0))
```

The `x_multi` argument is useful when the equation expects minutes but the analysis passes hours, or the reverse.
