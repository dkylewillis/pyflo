# Data Resources

The repository includes reusable CSV and JSON data in the `resources` directory. These files support the examples and tests, and they are useful starting points for local analyses.

## CSV Distributions

CSV distribution files are two-column ordered-pair datasets. Load them with `pyflo.system.array_from_csv` when you need a NumPy array.

```python
from pyflo import system

runoff_dist = system.array_from_csv("./resources/distributions/runoff/scs484.csv")
```

Main CSV groups:

| Folder | Contents |
| --- | --- |
| `resources/distributions/rainfall` | Dimensionless rainfall distributions and agency-specific rainfall patterns. |
| `resources/distributions/runoff` | Unit hydrograph runoff distributions such as SCS and Gamma shapes. |

Use `pyflo.distributions.increment(array, interval)` to interpolate distributions to a regular time step.

## Intensity JSON

Intensity files in `resources/intensity` store equation metadata and zone or frequency coefficients. A typical file contains:

| Key | Description |
| --- | --- |
| `equation` | Equation string consumed by `distributions.Evaluator`. |
| `input_key` | Variable name in the equation that receives the x value. |
| `zones` | Named regions with frequency-specific coefficients. |

```python
import json

from pyflo import distributions

with open("./resources/intensity/fdot.json") as data_file:
    data = json.load(data_file)

zone = next(item for item in data["zones"] if item["name"] == "FDOT ZONE 10")
freq = next(item for item in zone["frequencies"] if item["duration"] == 3)

intensity = distributions.Evaluator(
    equation=data["equation"],
    x_key=data["input_key"],
    eq_kwargs=freq["coefficients"],
    x_multi=60.0,
)
```

## Inlet, Splash, And Control Data

Other resource folders include inlet control and splash velocity data. They are plain JSON files, so load them with the standard `json` module and pass values into the relevant model code as needed.

## System Helpers

`pyflo.system` contains small CSV helpers:

| Function | Description |
| --- | --- |
| `tuple_list_from_csv(filename)` | Read rows into a list of float tuples. |
| `array_from_csv(filename)` | Read rows into a NumPy array. |
| `csv_from_tuple_list(filename, data)` | Write a list of tuples to CSV. |
