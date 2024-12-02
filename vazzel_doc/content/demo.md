# Human Mesh Recovery (HMR) Demo

## Overview

This script demonstrates how to use **HMR** (Human Mesh Recovery) to predict a 3D mesh of a human from a 2D image. The input is an image containing a person, optionally accompanied by OpenPose keypoint data to enhance the bounding box selection.

### Key Features
1. **Bounding Box Handling**:
   - If no OpenPose data is provided, the script assumes the image is centered on a person.
   - Optionally, OpenPose keypoints can help adjust the bounding box.

2. **3D Mesh Prediction**:
   - Predicts 3D joints and vertices using HMR.
   - Outputs both the skeletal structure and the complete 3D mesh.

3. **Visualization**:
   - Displays multiple views of the predicted 3D mesh:
     - Overlay on the original image.
     - Different virtual perspectives (e.g., ±60° rotations).

---

## Script Usage

The script can be executed from the command line with the following options:

### 1. On Images Centered on a Person
```bash
python -m demo --img_path data/im1963.jpg
```

### 2. On Images with OpenPose Output
```bash
python -m demo --img_path data/random.jpg --json_path data/random_keypoints.json
```

## Workflow Explanation

### 1. Image Preprocessing
- Input images are resized and cropped to center the person.
- Normalizes pixel values to the range [-1, 1].

### 2. 3D Mesh Prediction
The HMR Model predicts:
- Camera parameters
- Pose (72D vector)
- Shape (10D vector)

**Outputs**:
- 3D vertices
- Skeletal joints (2D and 3D).

### 3. Visualization
Results are rendered using SMPLRenderer, providing:
- Joint projections.
- 3D mesh overlay on the input image.
- Different viewpoints for the 3D mesh.

## Code Snippet
Here’s the core of the script:

```python
from src.RunModel import RunModel
from src.util import renderer as vis_util

def main(img_path, height, json_path=None):
    sess = tf.compat.v1.Session()
    model = RunModel(sess=sess)

    # Preprocess image
    input_img, proc_param, img = preprocess_image(img_path, json_path)
    input_img = np.expand_dims(input_img, 0)

    # Predict 3D mesh
    joints, verts, cams, joints3d, theta = model.predict(input_img, get_theta=True)

    # Visualize
    visualize(img, proc_param, joints, verts, cams)
```

## Visualization Example
Below is a sample of the rendered outputs for an input image:

- Input Image
- 2D Joint Projection
- 3D Mesh Overlay

## Future Integration
To integrate this script with other modules, such as `extract_measurements`, the mesh vertices and skeletal joints can be utilized for extracting anthropometric data.

## Notes
- TensorFlow compatibility is handled with `tf.compat.v1`.
- Input images should ideally be centered on a single person for best results.
