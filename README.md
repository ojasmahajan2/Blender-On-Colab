# Blender on Colab

A self-contained Jupyter notebook designed to render Blender scenes efficiently on Google Colab. It automates environment setup, addon installation, and hardware configuration to streamline the cloud rendering process. It is fully backward and forward compatible with both Blender 4.x and Blender 5.x.

## Features

- **Automated Setup**: Downloads and extracts the latest stable Blender release automatically if a local archive is not provided.
- **Addon Integration**: Automatically extracts and installs custom `.zip` addons from Google Drive before rendering. It dynamically detects and prints the internal addon module names so you can easily copy-paste them into the UI.
- **Hardware Acceleration**: Detects and enables OptiX for NVIDIA GPUs (such as Colab's T4, V100, or A100) to reduce render times. Falls back to CUDA if OptiX is unavailable. Supports multi-GPU rendering.
- **Auto-Resume**: Detects existing frames in the output directory and automatically resumes animation renders from the last completed frame in the event of a runtime disconnection.
- **Automated Compositing**: Injects a version-aware node setup to simultaneously export the base render, noisy render, and final composite. Seamlessly handles Blender 5.0 API changes (e.g., `NodeGroupOutput`, `OPEN_EXR_MULTILAYER`).
- **Performance Optimizations**: Injects `use_persistent_data` to keep BVH/textures in VRAM between frames (massively accelerating animation renders) and enables the OptiX denoiser for lightning-fast noise removal.
- **Colab UI**: Uses Colab Forms for a clean parameter interface, allowing users to configure render settings without directly modifying code.

## Prerequisites

- A Google account with access to Google Colab and Google Drive.
- A prepared `.blend` project file.

## Setup Instructions

1. Upload `BlenderOnColab.ipynb` to your Google Drive and open it via Google Colab.
2. Place your `.blend` project file into a folder named `Colab-Render` in the root of your Google Drive.
3. *(Optional)* Place any custom addon `.zip` files into `Colab-Render/addons`.

*Note: The notebook will automatically generate all other necessary directories for your renders.*

## Usage

1. Open the notebook in Google Colab.
2. Ensure a GPU runtime is active by navigating to **Runtime > Change runtime type > Hardware accelerator > T4 GPU (or any preferred GPU by choice)**.
3. Execute the initial cells to mount your Google Drive and set up the Blender environment.
4. Navigate to the **Configure & Render** cell. Fill out the form parameters on the right side of the screen, including:
   - Your `.blend` file name.
   - The render engine (CYCLES / EEVEE).
   - Frame range for animations.
   - Any addon module names you need enabled.
5. Run the cell to begin rendering. Your rendered frames will be saved directly to the `Colab-Render/Image Sequence/` folder in your Google Drive.

## Notes
- The notebook generates a local Python script on the Colab instance to configure the GPU and rendering parameters, making external setup scripts unnecessary.
- The generated setup script is highly robust. It will dynamically detect your Blender version (4.x or 5.x) and use the correct Python API to prevent crashes.
