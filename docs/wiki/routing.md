# Routing

The `pyflo.routing` module performs storage-based time-step simulations. It is most commonly used for reservoir bleed-down through an outlet link.

## Reservoirs

`routing.Reservoir` stores elevation-area contour data and solves area, storage, and stage.

```python
from pyflo import routing

contours = [
    (16.0, 0.10 * 43560.0),
    (21.5, 0.42 * 43560.0),
    (23.5, 0.61 * 43560.0),
    (29.8, 1.25 * 43560.0),
]
reservoir = routing.Reservoir(contours=contours, start_stage=25.35)

storage = reservoir.storage(25.35)
stage = reservoir.stage(storage)
```

The contour list is sorted by elevation on initialization. Area and storage can extrapolate above the highest contour. Storage below the lowest contour is zero.

## Tailwater

`routing.Tailwater` stores `(time, stage)` pairs and interpolates stage by time.

```python
tailwater = routing.Tailwater([(0.0, 4.0), (2.0, 4.5), (6.0, 4.1)])
stage = tailwater.stage(1.0)
```

All supplied times must be non-negative.

## Bleed-Down Workflow

Create a network with a reservoir on an upstream node and a weir or orifice outlet to a downstream node.

```python
from pyflo import networks, routing, sections

network = networks.Network()
pond_node = network.create_node()
outfall = network.create_node()

pond_node.add_reservoir(reservoir)

opening = sections.Circle(diameter=3.25 / 12.0)
outlet = pond_node.create_weir(
    node_2=outfall,
    invert=23.5,
    k_orif=0.6,
    k_weir=3.2,
    section=opening,
)

analysis = routing.Analysis(
    node=outfall,
    tw=0.0,
    duration=2.0,
    interval=5.0 / 60.0,
)
results = analysis.node_solution_results()
```

The result for each link contains a `data` list. Each row includes `time`, `inflow`, `outflow`, `storage`, and `stage`.

```python
for row in results[outlet]["data"]:
    print(row["time"], row["stage"], row["outflow"])
```

## Rainfall-Driven Routing

`routing.Analysis` accepts an optional `rain_dist`. If a link's upstream node has a basin and a rainfall distribution is supplied, PyFlo generates a basin flood hydrograph and uses it as inflow at each time step.

Keep `duration` and `interval` long enough to cover the generated hydrograph rows used by the simulation.
