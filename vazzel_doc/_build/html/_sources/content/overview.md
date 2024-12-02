# Human Body Measurement Using Computer Vision & 3D Modeling

## Overview

This system leverages advanced computer vision techniques and machine learning frameworks such as **OpenCV** and **TensorFlow** to extract accurate anthropometric measurements from a single image of a human subject. By analyzing key body landmarks, the system constructs a 3D model of the body and calculates various measurements, including arm length, waist circumference, chest size, and more, providing these metrics in centimeters.

### Key Features:

- **Body Landmark Detection:** Utilizes computer vision algorithms to identify key landmarks on the human body, such as joints, waist, hips, and shoulders.
- **3D Model Construction:** Based on the detected landmarks, a 3D model of the body is generated, enabling precise calculation of body measurements.
- **Measurement Extraction:** Extracts multiple anthropometric measurements, including height, waist, chest, arm length, shoulder width, and more.
- **Real-Time Input Processing:** Processes an input image in real-time to produce body measurements, requiring only the height of the person for scaling.

### How It Works:

1. **Input Image:** The system requires a single image of the subject where their body is fully visible and standing upright. 
2. **Landmark Detection:** Key body landmarks are detected and processed by the model.
3. **3D Model Creation:** Using the detected landmarks, a 3D model of the subject’s body is constructed.
4. **Measurement Calculation:** The system computes various body measurements in centimeters, providing an accurate representation of the subject’s body dimensions.

### Supported Measurements:
- **Height**
- **Waist circumference**
- **Belly circumference**
- **Chest circumference**
- **Wrist circumference**
- **Neck circumference**
- **Arm length**
- **Thigh circumference**
- **Shoulder width**
- **Hip circumference**
- **Ankle circumference**

This project is designed to assist in applications requiring body measurements, such as online fashion fitting, medical assessments, fitness tracking, and more.
