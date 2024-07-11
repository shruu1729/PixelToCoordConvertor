# Pixel to Coordinates Converter
4

## Description
This Python script converts pixel locations in an image to coordinates in various coordinate systems. It supports multiple image formats, including GIFs, and provides visualization of the converted coordinates.
There are two version of this code in the first version the image is saved in PNG format and in the second version the output image is saved in its original format.

# The image is saved as PNG instead of the original format for a few reasons:

**Quality preservation:** PNG is a lossless format, which means it preserves image quality without compression artifacts. This is especially important for the visualized image, which contains added elements like circles and text.

**Consistency:** By converting to PNG, the script ensures a consistent output format regardless of the input format (which could be JPG, GIF, BMP, etc.). This simplifies the handling of the output file.

**Compatibility:** PNG is widely supported and can be opened on virtually any device or platform, ensuring the user can easily view the result.

**Handling of modifications:** The script adds visual elements to the image. Saving in PNG ensures these additions are preserved accurately, which might not be the case with some other formats (especially lossy formats like JPEG).

**GIF limitations:** If the original image was a GIF, especially an animated one, saving the modified single frame back as a GIF could be problematic or lose the modifications.

However, if we prefer to save the image in its original format, we could modify the code to do so. This might lead to quality loss for some formats or issues with preserving the added visual elements.

## Approach

1. The script uses PIL (Python Imaging Library) to handle various image formats, including GIFs.
2. It supports three coordinate systems: top-left, center, and bottom-left.
3. For efficiency, it uses NumPy for bulk conversions and multiprocessing for parallel processing of large datasets.
4. The `convert_pixel_to_coords` function is cached using `lru_cache` for single pixel conversions.
5. The script visualizes the conversions by drawing circles at pixel locations and labeling them with the converted coordinates.

## Features
- Supports multiple coordinate systems: top-left, center, and bottom-left
- Handles various image formats, including GIFs (processes the first frame for animated GIFs)
- Visualizes converted coordinates on the image
- Efficiently processes multiple pixel locations using parallel computing
- Saves and downloads the visualized image in PNG format
- Preserves original image colors in visualization

## Complexity Analysis and Optimizations

### Time Complexity

- Single pixel conversion: O(1) due to the use of @lru_cache
- Bulk conversion: O(n) where n is the number of pixel locations
- Parallel processing: O(n/p) where p is the number of processes, improving performance for large datasets

### Space Complexity

- O(n) where n is the number of pixel locations, as we store the input pixels and converted coordinates

### Optimizations

1. **Caching**: 
   - The `convert_pixel_to_coords` function uses `@lru_cache` to store results of previous calculations, reducing redundant computations for repeated pixel locations.

2. **NumPy for Bulk Operations**: 
   - The `convert_pixels_to_coords_np` function uses NumPy arrays for efficient bulk conversions, leveraging vectorized operations which are significantly faster than Python loops.

3. **Parallel Processing**: 
   - The `parallel_convert` function uses multiprocessing to distribute the workload across multiple CPU cores, significantly speeding up conversions for large datasets.

4. **Efficient Image Handling**:
   - Uses PIL (Python Imaging Library) for efficient image processing and supports various image formats including GIFs.
   - For GIFs, only the first frame is processed to save memory and computation time.

5. **Vectorized Boundary Checking**:
   - Uses NumPy's vectorized operations to efficiently check if any pixel is out of bounds, avoiding loops.

6. **Minimal Memory Usage**:
   - The script processes and visualizes the image in-place, avoiding creation of unnecessary copies of large image data.

7. **Efficient Coordinate System Conversion**:
   - Uses conditional statements instead of function calls for different coordinate systems, reducing function call overhead.

8. **Chunking in Parallel Processing**:
   - Divides the workload into chunks for parallel processing, balancing the load across available CPU cores.

These optimizations make the script efficient for both small and large datasets, with particularly significant performance improvements for bulk conversions on multi-core systems.

## Requirements
- Python 3.x
- OpenCV (cv2)
- NumPy
- Pillow (PIL)
- Google Colab environment (for file upload and download features)

## Installation
This script is designed to run in a Google Colab environment. No additional installation is required if running in Colab. For local use, ensure you have the required libraries installed:
```pip install opencv-python numpy pillow```

## How to Run

1. Open the script in a Google Colab notebook.
2. Run the script.
3. When prompted, upload an image file (JPG, PNG, GIF, etc.).
4. Enter the desired coordinate system (top-left, center, or bottom-left).
5. Input pixel locations when prompted. Enter X and Y coordinates as integers.
6. After entering pixel locations, the script will display the converted coordinates and show the visualized image.
7. Choose whether to save and download the visualized image.

## Example

```python
# Run the script in Google Colab
!python pixel_to_coords.py

# Sample interaction:
# Please upload an image file (including GIFs).
# [Upload an image named 'example.jpg']
# Enter coordinate system (top-left/center/bottom-left): center
# Enter the X coordinate of the pixel: 100
# Enter the Y coordinate of the pixel: 100
# Enter another pixel location? (yes/no): no
# Pixel location (100, 100) corresponds to coordinates: x=-60, y=140
# [Displays visualized image]
# Save and download the visualized image? (yes/no): yes
# Image saved and downloaded as visualized_example.png
```

## Assumptions
1. The script assumes that the input image is a valid image file that can be opened by PIL.
2. Pixel coordinates outside the image bounds will raise an error.
3. For GIF images, only the first frame is processed.
4. The script assumes that the user has the necessary permissions to upload files and download the output image.
5. It's assumed that the Google Colab environment is used, as the script utilizes `google.colab.files` for file handling.
6. The visualized image is saved in PNG format to preserve quality.(However in version 2 you can save your output image in original format if you don't want to save it in PNG format.)

## Error Handling
- The script checks for pixel coordinates outside the image bounds and raises an error if detected.
- Invalid coordinate system inputs default to 'top-left'.
- Other unexpected errors are caught and reported to the user.s



