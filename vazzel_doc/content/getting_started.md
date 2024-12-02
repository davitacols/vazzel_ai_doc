# Getting Started

Welcome to the Human Body Measurement project documentation! This section will guide you through setting up the project and resolving any known issues.

## Prerequisites

Before you begin, ensure you have the following installed:
- Python 3.10 or later
- Virtual environment tools (`venv` or `virtualenv`)
- Basic command-line tools

## Setup Guide

### Step 1: Create a Virtual Environment

Run the following command to create a virtual environment for the project:

```bash
python -m venv venv
```

Activate the virtual environment:
* **On Windows**:
```bash
venv\Scripts\activate
```

* **On macOS/Linux**:
```bash
source venv/bin/activate
```

### Step 2: Install Dependencies

Install all required Python packages using the `requirements.txt` file:

```bash
pip install -r requirements.txt
```

### Step 3: Install OpenDR

To ensure the `opendr` library is available, navigate to the `opendr` directory and run:

```bash
cd path_to_your_project\HBM\opendr
python setup.py install
```

### Step 4: Fix Known Issues

#### Chumpy Compatibility Fix

Locate the following file in your virtual environment:

```
...\venv\Lib\site-packages\chumpy-0.70-py3.10.egg\chumpy\__init__.py
```

Comment out line 11:

```python
# from numpy import bool, int, float, complex, object, unicode, str, nan, inf
```

Save the file and restart your environment if necessary.

### Verifying Installation

Run the following command to ensure all dependencies are correctly installed:

```bash
python -c "import tensorflow; import cv2; print('Setup complete!')"
```