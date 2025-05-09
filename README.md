

**Text/Image to 3D Model Generator**

This project converts a text prompt or image into a 3D model (.obj and .ply format) using Stable Diffusion, depth estimation, and point cloud processing.

**Steps to run:**

1. **Recommended Environment:**

Run this script in Google Colab or Kaggle Kernel for ease of use and free GPU access.

* Go to Runtime > Change runtime type
* Set Hardware Accelerator to GPU

1. Choose whether to give Text as Input or Image as Input for creating a 3D Model.

* Press 1 → enter a descriptive **text prompt**
* Press 2 → upload your **image file**

1. The required libraries have to be installed before running the code. These pre required packages are given in the starting cells.
2. After installing all the packages, restart the session.
3. Run each cell of the notebook and at end download the .obj file generated to see the output.
4. To view the 3D model, use the below link website.

[Free Online Tool to View 3D OBJ Files Online - ImageToStl](https://imagetostl.com/view-obj-online)

**Libraries Used:**

* diffusers – Text-to-image generation
* rembg – Background removal
* transformers – Depth estimation
* torch – GPU inference
* open3d – 3D point cloud operations
* PIL, matplotlib, numpy, os, shutil – Utilities

**Thought Process:**

This project is built to demonstrate the capability of AI models to generate 3D data from either text prompts or input images, using a combination of deep learning pipelines. Here's a breakdown of each core step:

**1. Image Acquisition**

Input Options: The user can either enter a text or upload an image file.

Image Generation: When a text prompt is provided, the script uses the Stable Diffusion XL model from Hugging Face to generate a high-resolution image (1024x1024 px).

**2. Background Removal**

To isolate the main subject from the background, which helps in producing cleaner and more accurate depth maps and 3D models.

Tool Used: rembg library is used to remove the background.

Output: A transparent-background image of the main subject (output.png).

**3. Depth Estimation**

Model Used: **ZoeDepth** by Intel (Intel/zoedepth-nyu-kitti) is used to estimate the depth map of the image.

Working: This transformer-based model predicts how far each pixel is from the camera, effectively understanding the 3D structure of the image.

Output: A grayscale depth map (depth\_map.png) where darker pixels are closer and lighter pixels are farther away.

**4. 3D Point Cloud Generation**

Open3D is used to convert the depth map + RGB image into a point cloud representation.

RGBD Image Creation: Combines the color image and depth map into a format that allows 3D reconstruction.

Camera Intrinsics: Default values (fx = fy = 500) are used to simulate a virtual camera's focal length and center. These values can be fine-tuned for better results if real camera data is available.

Output: A .ply file (point\_cloud.ply) containing the 3D point cloud.

**5. File Size Handling and Compression**

3D point clouds can be large (especially for high resolutions Images), which can cause issues when viewing or sharing.

If the .ply file exceeds 100MB, it is downsampled using voxel grid filters with various sizes. This reduces the number of points while preserving the shape and structure of the object.

Result: A smaller .ply file (point\_cloud\_compressed\_\*.ply) with minimal loss in quality.

* 1. **Conversion to OBJ Format**

Since we are using **monocular images**, the generated point cloud includes background and lacks accurate 3D structure. Creating a mesh from this would result in irregular shapes, as proper mesh generation requires **stereo camera data**. So, instead of meshing, I simply changed the file extension from .ply to .obj for easier viewing in 3D model viewers.

