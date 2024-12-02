# Body Measurement Extraction Module

## Overview
This module provides functionality to calculate body measurements based on control points and vertex data. It includes methods for parsing control points from a file, calculating measurements, and extracting results for further processing.

## Functions

### `convert_cp()`
Parses control points from a file.

#### Description
Reads control points from a text file (`customBodyPoints.txt`) located in the `data/` directory. Each control point group is identified by a `#` separator in the file. The points are converted into lists of floats and grouped appropriately.

#### Returns
- `list[list[list[float]]]`: A nested list of control points.

#### Raises
- `FileNotFoundError`: If the control points file is not found.

#### Example
```python
control_points = convert_cp()
```

### `calc_measure(cp, vertices, height)`
Calculates body measurements based on control points and vertex data.

#### Parameters
- `cp` (list[list[list[float]]]): Control points parsed from `convert_cp`.
- `vertices` (np.ndarray): A NumPy array of shape (N, 3) containing vertex coordinates.
- `height` (float): Height of the model in meters.

#### Returns
- `np.ndarray`: A reshaped array of calculated measurements, scaled appropriately.

#### Description
Calculates distances between vertices or interpolated points based on control point types:
- **Type 1**: Direct vertex reference.
- **Type 2**: Weighted interpolation between two vertices.
- **Type 3**: Weighted interpolation between three vertices.

Scales the resulting measurements based on height and applies additional scaling factors for specific indices. Pads the array to ensure it matches the expected number of measurements (`utils.M_NUM`).

#### Raises
- `ValueError`: If vertices are missing or no valid measurements can be calculated.

#### Example
```python
measurements = calc_measure(cp, vertices, 1.75)
```

### `extract_measurements(height, vertices)`
Extracts body measurements given control points and vertex data.

#### Parameters
- `height` (float): Height of the model in meters.
- `vertices` (np.ndarray): A NumPy array of shape (N, 3) containing vertex coordinates.

#### Returns
- `np.ndarray`: Measurements for the model, scaled and formatted for output.

#### Description
Combines the control point parsing (`convert_cp`) and measurement calculation (`calc_measure`) into a single workflow. Prints measurement labels and values for debugging and verification.

#### Raises
- `ValueError`: If vertices are missing.
- `Exception`: For unexpected errors during measurement extraction.

#### Example
```python
vertices = np.random.rand(1000, 3)  # Replace with actual vertex data
measurements = extract_measurements(1.75, vertices)
```

## Workflow

### Parse control points from `customBodyPoints.txt`:
```python
cp = convert_cp()
```

### Use control points and vertex data to calculate measurements:
```python
measurements = calc_measure(cp, vertices, height)
```

### Extract and print measurements:
```python
extract_measurements(height, vertices)
```

## Key Notes
- The control points file must exist in the `data/` directory.
- The vertex array should be a valid NumPy array of shape (N, 3) where N is the number of vertices.
- Measurements are scaled based on the height parameter and additional scaling factors for specific indices.

## Error Handling
- Logs invalid points or vertices to the console.
- Skips invalid measurements but ensures valid ones are processed.
- Provides detailed error messages for missing files, invalid input, or unexpected runtime issues.

## Example Usage
```python
import numpy as np
import utils

# Sample data
vertices = np.random.rand(1000, 3)  # Replace with actual vertex data
height = 1.75  # Height in meters

try:
    # Extract and display measurements
    measurements = extract_measurements(height, vertices)
    print("Measurements successfully extracted!")
except Exception as e:
    print(f"Error: {e}")
```

