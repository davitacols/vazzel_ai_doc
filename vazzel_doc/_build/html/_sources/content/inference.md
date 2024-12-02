# Documentation for DeepLab Segmentation Script with GPU Support(inference.py)

## Overview
This script utilizes the DeepLab model to perform semantic segmentation with GPU acceleration. It removes the background from images, keeping only the subject of interest (e.g., a person), and provides an interface for further processing, such as height measurement or other analyses. The script supports dynamic GPU memory allocation and includes robust error handling.

## Features

### Semantic Segmentation
- Performs segmentation using the DeepLab model pre-trained on the Pascal VOC dataset.
- Supports identifying specific classes, such as "person."

### Background Removal
- Removes the background based on segmentation masks.
- Allows customization of the replacement background color.

### GPU Optimization
- Dynamically configures TensorFlow to utilize GPU memory efficiently.
- Falls back to CPU if no GPU is available or if GPU configuration fails.

### User Inputs
- Processes images provided via command-line arguments.
- Enables height customization for measurement.

## Requirements

### Prerequisites
- **Python**: 3.7 or later
- **Libraries**:
  - tensorflow-gpu
  - numpy
  - opencv-python
  - Pillow

Install dependencies using:
```bash
pip install tensorflow-gpu numpy opencv-python pillow
```

- **Model**: DeepLab pre-trained model (downloaded automatically if not available locally).

### Hardware
- **GPU with CUDA support** (recommended for faster processing).

## How to Use

### Command-Line Arguments
- `--input_dir`: Path to the input image file (required).
- `--height`: Height (integer) for measurement adjustments (required).

### Example Command
```bash
python script.py --input_dir ./input_image.jpg --height 170
```

### Output
- Processes the input image to remove the background.
- Calls the main function from the demo module with the processed image.

## Code Components

### 1. DeepLabModel Class
Handles loading and running the DeepLab model.
- **Initialization**: Loads the model from a `.tar.gz` file.
- **run Method**: Resizes the input image and runs inference to generate a segmentation map.

### 2. Utility Functions
- **create_pascal_label_colormap**: Creates a color map for visualizing segmentation results.
- **label_to_color_image**: Maps segmentation labels to colors.
- **remove_background**: Removes the background using a binary segmentation mask and replaces it with a specified color.

### 3. Main Processing Logic
- Loads the model and input image.
- Runs the segmentation model and generates a mask.
- Removes the background and calls the main function with the modified image.

## Error Handling

### Image Loading Errors
- Verifies the existence of the input image.
- Provides a clear error message if the file is missing or inaccessible.

### Model Download Errors
- Handles network errors during model download.
- Displays the HTTP error code and reason for easier troubleshooting.

### GPU Configuration Errors
- Logs issues with GPU initialization.
- Defaults to CPU usage in case of failures.

## File Structure
- **Input File**: An image in formats such as `.jpg`, `.png`.
- **Model Directory**:
  - Contains the downloaded DeepLab model (`.tar.gz` file).
  - Automatically created if not present.

## Troubleshooting

### No GPU Detected
- Ensure that a CUDA-compatible GPU is installed.
- Check if TensorFlow is correctly installed for GPU support (`tensorflow-gpu`).

### Slow Processing
- Confirm GPU utilization using tools like `nvidia-smi`.
- Resize the input image to reduce computational load.

### Model Download Fails
- Ensure network connectivity.
- Verify the URL or manually download the model from TensorFlow’s model zoo.

## Advanced Configuration

### Custom Background Colors
Modify the `replacement_color` in the `remove_background` function to change the background color.
```python
replacement_color = (0, 0, 0)  # Black background
```

### Segmentation for Other Classes
Modify the `mask_sel` logic to target a different class by adjusting the label index (e.g., 15 for "person").
```python
mask_sel = (seg_map_resized == CLASS_INDEX).astype(np.uint8) * 255
```

# Classes

## 1. `DeepLabModel`
This class is responsible for loading the DeepLab model and performing semantic segmentation on input images.

### Attributes
- **`INPUT_TENSOR_NAME`**: TensorFlow input tensor name for feeding the image (`ImageTensor:0`).
- **`OUTPUT_TENSOR_NAME`**: TensorFlow output tensor name for retrieving predictions (`SemanticPredictions:0`).
- **`INPUT_SIZE`**: Default size (513) for resizing input images.
- **`FROZEN_GRAPH_NAME`**: Name of the frozen inference graph (`frozen_inference_graph`).
- **`graph`**: TensorFlow computation graph for running the model.
- **`sess`**: TensorFlow session for executing the graph.

### Methods
#### `__init__(self, tarball_path)`
Initializes the model by loading a pre-trained DeepLab graph from a tarball file.
- **Parameters**:
  - `tarball_path`: Path to the tarball containing the frozen inference graph.
- **Raises**:
  - `RuntimeError`: If the graph cannot be found in the tarball.

#### `run(self, image)`
Runs inference on an input image to produce a segmentation map.
- **Parameters**:
  - `image`: A `PIL.Image` object representing the input image.
- **Returns**:
  - `resized_image`: The resized image used for inference.
  - `seg_map`: The segmentation map as a NumPy array.

---

# Functions

## 1. `create_pascal_label_colormap()`
Generates a colormap for visualizing segmentation results.

### Description
Creates a Pascal VOC colormap where each class is assigned a unique color.

### Returns
- A NumPy array of shape `(256, 3)` representing the RGB colormap.

## 2. `label_to_color_image(label)`
Maps segmentation labels to their corresponding colors using the colormap.

### Description
Converts a 2D label map into an RGB image using the Pascal VOC colormap.

### Parameters
- `label`: A 2D NumPy array of segmentation labels.

### Returns
- A 3D NumPy array representing the colorized image.

### Raises
- `ValueError`: If the input label is not 2D or contains values outside the colormap range.

## 3. `remove_background(image, mask, replacement_color=(255, 255, 255))`
Removes the background from an image using a binary segmentation mask.

### Description
Segments the foreground (e.g., person) based on the provided mask and replaces the background with a solid color.

### Parameters
- `image`: A NumPy array representing the input image.
- `mask`: A binary mask where non-zero pixels represent the foreground.
- `replacement_color`: A tuple `(R, G, B)` defining the background color. Defaults to white.

### Returns
- A NumPy array representing the image with the background removed.

---

# Command-Line Interface

## Parser
Uses `argparse` for user inputs via command-line arguments.

### Arguments
- `--input_dir (-i)`: Path to the input image file (required).
- `--height (-ht)`: Height (integer) for measurement adjustments (required).

---

# Global Variables

## 1. `LABEL_NAMES`
A NumPy array containing the names of the Pascal VOC dataset classes.

## 2. `FULL_LABEL_MAP`
A NumPy array reshaped to map class indices to their corresponding labels.

## 3. `FULL_COLOR_MAP`
A colorized version of the label map for visualization.

## 4. `MODEL_NAME`
Specifies the DeepLab model variant to use (`xception_coco_voctrainval`).

## 5. `_DOWNLOAD_URL_PREFIX`
URL prefix for downloading the pre-trained DeepLab models.

## 6. `_MODEL_URLS`
A dictionary mapping model names to their respective download URLs.

## 7. `_TARBALL_NAME`
Name of the tarball file for the selected model.

---

# Processing Flow

### Model Setup
- Checks if the model is available locally.
- Downloads the pre-trained model if not found.
- Initializes the `DeepLabModel` class with the model file.

### Input Image Processing
- Loads the input image using Pillow.
- Runs the `DeepLabModel`'s `run` method to obtain a segmentation map.

### Segmentation Mask Application
- Resizes the segmentation map to match the original image size.
- Extracts the binary mask for the "person" class.

### Background Removal
- Calls `remove_background` to replace the background with a solid color.

### Further Processing
- Passes the processed image to the main function from the demo module.

---

# Notes for Developers

### Adding Support for New Classes
- Modify the segmentation mask logic to include additional class indices as needed.
- Update the `LABEL_NAMES` and corresponding processing logic.

### Custom Backgrounds
- Replace `replacement_color` in `remove_background` with your desired RGB value.

### GPU Configuration
- Ensure the system has CUDA-compatible GPUs and the correct version of `tensorflow-gpu`.

### Extending Functionality
- Integrate the processed images into other workflows, such as object detection or pose estimation.


## Notes
- The script uses TensorFlow 2.x for GPU optimization and supports backward compatibility with TensorFlow 1.x graph definitions.
- Ensure your system has enough GPU memory for large images or consider resizing images beforehand.

## References
- [DeepLab: Semantic Image Segmentation](https://github.com/tensorflow/models/tree/master/research/deeplab)
- [Pascal VOC Dataset](http://host.robots.ox.ac.uk/pascal/VOC/)
